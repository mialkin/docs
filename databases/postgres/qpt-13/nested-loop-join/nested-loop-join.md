# Nested loop join

## Table of contents

- [Nested loop join](#nested-loop-join)
  - [Table of contents](#table-of-contents)
  - [Соединение](#соединение)
  - [Nested loop](#nested-loop)
  - [Модификации](#модификации)
  - [В параллельных планах](#в-параллельных-планах)
  - [Стоимость соединения вложенным циклом](#стоимость-соединения-вложенным-циклом)
  - [Итоги](#итоги)

## Соединение

Способы соединения — не соединения SQL. `INNER`/`LEFT`/`RIGHT`/`FULL`/`CROSS JOIN`, `IN`, `EXISTS` — логические операции. Способы соединения — механизм реализации этих логических операций.

Соединяются не таблицы, а наборы строк. Наборы строк могут быть получены от любого узла дерева плана.

Наборы строк соединяются попарно, то есть несколько наборов строк не могут соединяться одновременно. Порядок соединений важен с точки зрения производительности. Обычно важен и порядок внутри пары наборов.

## Nested loop

Смысл и идея соединения вложенным циклом: для каждой строки одного набора перебираем строки другого набора, и ищем совпадения по условию соединения.

Обычно хорошо, когда внешний набор, по которому мы делаем внешний цикл, состоит из небольшого количества записей.

С другой стороны, желательно, чтобы второй набор был тоже маленьким, чтобы мы могли его быстро перебрать, либо чтобы он имел индекс по условию соединения. Индекс позволит не просматривать второй набор полностью, а находить нужные строчки используя индекс.

<img src="nested_loop.jpeg" width="600px" alt="Соединение вложенным циклом"/>

<br />

Так выглядит план для соединения вложенным циклом, которое оптимизатор обычно предпочитает для небольших выборок (смотрим перелеты, включенные в два билета):

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM tickets t
         JOIN ticket_flights tf ON tf.ticket_no = t.ticket_no
WHERE t.ticket_no IN ('0005432312163', '0005432312164');
```

```console
Nested Loop                                                                      
  ->  Index Scan using tickets_ticket_no_book_ref_idx on tickets t               
        Index Cond: (ticket_no = ANY ('{0005432312163,0005432312164}'::bpchar[]))
  ->  Index Scan using ticket_flights_pkey on ticket_flights tf                  
        Index Cond: (ticket_no = t.ticket_no)                                    
```

Это параметризованное соединение. Для каждой строки внешнего набора (узел Index Scan по таблице билетов) выбираются строки из внутреннего набора (узел Index Scan по таблице перелетов), отвечающие условию соединения. На каждой итерации внешнего цикла условие индексного доступа для внутреннего набора имеет вид `ticket_no` = константа.

Процесс повторяется до тех пор, пока внешний набор не исчерпает все строки.

Команда `EXPLAIN ANALYZE` позволяет узнать, сколько раз на самом деле выполнялся вложенный цикл (loops) и сколько в среднем было выбрано строк (rows) и потрачено времени (time) за один раз. Видно, что планировщик немного ошибся - получилось 8 строк вместо 6.

```sql
EXPLAIN (ANALYZE, SUMMARY OFF)
SELECT *
FROM tickets t
         JOIN ticket_flights tf ON tf.ticket_no = t.ticket_no
WHERE t.ticket_no IN ('0005432312163', '0005432312164');
```

```console
Nested Loop  (cost=0.99..46.11 rows=6 width=136) (actual time=0.034..0.061 rows=8 loops=1)                                                       
  ->  Index Scan using tickets_ticket_no_book_ref_idx on tickets t  (cost=0.43..12.90 rows=2 width=104) (actual time=0.018..0.027 rows=2 loops=1)
        Index Cond: (ticket_no = ANY ('{0005432312163,0005432312164}'::bpchar[]))                                                                
  ->  Index Scan using ticket_flights_pkey on ticket_flights tf  (cost=0.56..16.58 rows=3 width=32) (actual time=0.011..0.014 rows=4 loops=2)    
        Index Cond: (ticket_no = t.ticket_no)                                                                                                    
```

Предупреждение: вывод времени выполнения каждого шага, как в этом примере, может существенно замедлять выполнение запроса на некоторых платформах. Если такая информация не нужна, лучше указывать timing off.

## Модификации

Существует несколько модификаций алгоритма. Для левого соединения:

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM aircrafts a
         LEFT JOIN seats s ON (a.aircraft_code = s.aircraft_code)
WHERE a.model LIKE 'Аэробус%';
```

```console
Nested Loop Left Join                                       
  ->  Seq Scan on aircrafts_data ml                         
        Filter: ((model ->> lang()) ~~ 'Аэробус%'::text)    
  ->  Bitmap Heap Scan on seats s                           
        Recheck Cond: (ml.aircraft_code = aircraft_code)    
        ->  Bitmap Index Scan on seats_pkey                 
              Index Cond: (aircraft_code = ml.aircraft_code)
```

Эта модификация возвращает строки, даже если для левого (а) набора строк не нашлось соответствия в правом (s) наборе.

Такая же модификация есть и для правого соединения. Но надо помнить, что планировщик сам определяет порядок, в котором соединяются таблицы, независимо от того, как они перечислены в запросе.

Антисоединение возвращает строки одного набора, если только для них не нашлось соответствия в другом наборе. Такая модификация может использоваться для обработки предиката `NOT EXISTS`:

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM aircrafts a
WHERE a.model LIKE 'Аэробус%'
  AND NOT EXISTS (SELECT *
                  FROM seats s
                  WHERE s aircraft_code = a aircraft_code);
```

```console
Nested Loop Left Join                                       
  ->  Seq Scan on aircrafts_data ml                         
        Filter: ((model ->> lang()) ~~ 'Аэробус%'::text)    
  ->  Bitmap Heap Scan on seats s                           
        Recheck Cond: (ml.aircraft_code = aircraft_code)    
        ->  Bitmap Index Scan on seats_pkey                 
              Index Cond: (aircraft_code = ml.aircraft_code)
```

Та же операция антисоединения используется и для аналогичного запроса, записанного иначе:

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM aircrafts a
         LEFT JOIN seats s ON (a.aircraft_code = s.aircraft_code)
WHERE a.model LIKE 'Аэробус%'
  AND s.aircraft_code IS NULL;
```

```console
Nested Loop Anti Join                                   
  ->  Seq Scan on aircrafts_data ml                     
        Filter: ((model ->> lang()) ~~ 'Аэробус%'::text)
  ->  Index Scan using seats_pkey on seats s            
        Index Cond: (aircraft_code = ml.aircraft_code)  
```

Для предиката `EXISTS` может использоваться полусоединение, которое возвращает строки одного набора, если для них нашлось хотя бы одно соответствие в другом наборе:

```sql
EXPLAIN
SELECT *
FROM aircrafts a
WHERE a.model LIKE 'Аэробус%'
  AND EXISTS (SELECT *
              FROM seats s
              WHERE s.aircraft_code = a.aircraft_code);
```

```console
Nested Loop Semi Join  (cost=0.28..4.02 rows=1 width=52)                             
  ->  Seq Scan on aircrafts_data ml  (cost=0.00..3.39 rows=1 width=52)               
        Filter: ((model ->> lang()) ~~ 'Аэробус%'::text)                             
  ->  Index Only Scan using seats_pkey on seats s  (cost=0.28..6.88 rows=149 width=4)
        Index Cond: (aircraft_code = ml.aircraft_code)                               
```

Обратите внимание: хотя в плане для таблицы `s` указано `rows=149`, на самом деле достаточно получить всего одну строку, чтобы понять значение предиката `EXISTS`.

PostgreSQL так и делает (actual rows):

```sql
EXPLAIN(ANALYZE, COSTS OFF, TIMING OFF, SUMMARY OFF)
SELECT *
FROM aircrafts a
WHERE a.model LIKE 'Аэробус%'
  AND EXISTS (SELECT *
              FROM seats s
              WHERE s.aircraft_code = a.aircraft_code);
```

```console
Nested Loop Semi Join (actual rows=3 loops=1)                            
  ->  Seq Scan on aircrafts_data ml (actual rows=3 loops=1)              
        Filter: ((model ->> lang()) ~~ 'Аэробус%'::text)                 
        Rows Removed by Filter: 6                                        
  ->  Index Only Scan using seats_pkey on seats s (actual rows=1 loops=3)
        Index Cond: (aircraft_code = ml.aircraft_code)                   
        Heap Fetches: 0                                                  
```

Модификации алгоритма вложенного цикла для полного соединения (`FULL JOIN`) не существует. Это связано с тем, что полный проход по второму набору строк может не выполняться.

Если пренебречь производительностью, полное соединение можно получить, объединив левое соединение и антисоединение. Такой способ может пригодиться, так как `FULL JOIN` (как мы увидим позже) работает только с эквисоединениями (`=`).

## В параллельных планах

Метод соединения вложенным циклом может использоваться и в параллельных планах: внешний набор строк сканируется параллельно, внутренний — последовательно каждым рабочим процессом.

## Стоимость соединения вложенным циклом

Посмотрим на стоимость следующего плана выполнения:

```sql
EXPLAIN
SELECT *
FROM tickets t
         JOIN ticket_flights tf ON tf.ticket_no = t.ticket_no
WHERE t.ticket_no IN ('0005432312163', '0005432312164');
```

```console
Nested Loop  (cost=0.99..46.11 rows=6 width=136)                                                       
  ->  Index Scan using tickets_ticket_no_book_ref_idx on tickets t  (cost=0.43..12.90 rows=2 width=104)
        Index Cond: (ticket_no = ANY ('{0005432312163,0005432312164}'::bpchar[]))                      
  ->  Index Scan using ticket_flights_pkey on ticket_flights tf  (cost=0.56..16.58 rows=3 width=32)    
        Index Cond: (ticket_no = t.ticket_no)                                                          
```

Начальная стоимость узла Nested Loop — сумма начальных стоимостей дочерних узлов (Index Scan).

Полная стоимость узла Nested Loop складывается из:

- стоимости получения данных от внешнего набора (полная стоимость узла Index Scan по билетам)
- стоимости получения данных от внутреннего набора (полная стоимость узла Index Scan по перелетам), умноженной на число строк внешнего набора (2)

В общем случае формула более сложная, но основной вывод: стоимость пропорциональна $N \times М$, где `N` - число строк во внешнем наборе данных, а `М` - среднее число строк, внутреннего набора, читаемое за одну итерацию. В худшем случае стоимость получается квадратичной.

## Итоги

- Вложенный цикл не требует подготовительных действий: может отдавать результат соединения без задержек
- Эффективен для небольших выборок, когда внешний набор строк не очень велик, а к внутреннему есть эффективный доступ (обычно по индексу)
- Зависит от порядка соединения. Обычно лучше, если внешний набор меньше внутреннего
- Поддерживает соединение по любому условию: как эквисоединения (`=`), так и любые другие
