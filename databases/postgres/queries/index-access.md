# Index access

- B-trees
- Index scan
- Index only scan
- Indexes with additional columns
- Duplicates in index

## Table of contents

- [Index access](#index-access)
  - [Table of contents](#table-of-contents)
  - [Сканирование индекса](#сканирование-индекса)
    - [Поиск по диапазону](#поиск-по-диапазону)
    - [Параллельное сканирование индекса](#параллельное-сканирование-индекса)
  - [Число рабочих процессов](#число-рабочих-процессов)

## Сканирование индекса

Рассмотрим таблицу бронирований:

```bash
psql -U postgres
\c demo
\d bookings
```

```console
                        Table "bookings.bookings"
    Column    |           Type           | Collation | Nullable | Default
--------------+--------------------------+-----------+----------+---------
 book_ref     | character(6)             |           | not null |
 book_date    | timestamp with time zone |           | not null |
 total_amount | numeric(10,2)            |           | not null |
Indexes:
    "bookings_pkey" PRIMARY KEY, btree (book_ref)
Referenced by:
    TABLE "tickets" CONSTRAINT "tickets_book_ref_fkey" FOREIGN KEY (book_ref) REFERENCES bookings(book_ref)
```

Столбец `book_ref` является первичным ключом и для него автоматически был создан индекс `bookings_pkey`.

Проверим план запроса с поиском одного значения:

```sql
EXPLAIN
SELECT *
FROM bookings
WHERE book_ref = 'CDE08B';
```

```console
Index Scan using bookings_pkey on bookings  (cost=0.43..8.45 rows=1 width=21)
  Index Cond: (book_ref = 'CDE08B'::bpchar)
```

Выбран метод доступа Index Scan, указано имя использованного индекса. Здесь обращение и к индексу, и к таблице представлено одним узлом плана. Строкой ниже указано условие доступа.

Начальная стоимость индексного доступа — оценка ресурсов для спуска к листовому узлу. Она зависит от логарифма количества листовых узлов (количество операций сравнения, которые надо выполнить) и от высоты дерева. При оценке считается, что необходимые страницы окажутся в кеше, и оцениваются только ресурсы процессора: цифра получается небольшой.

Полная стоимость добавляет оценку чтения необходимых листовых страниц индекса и табличных страниц.

В данном случае, поскольку индекс уникальный, будет прочитана одна индексная страница и одна табличная. Стоимость каждого из чтений оценивается параметром `random_page_cost`:

```sql
SELECT current_setting('random_page_cost');
-- 4
```

Его значение обычно больше, чем `seq_page_cost`, поскольку произвольный доступ стоит дороже (хотя для SSD-дисков этот параметр следует существенно уменьшить).

Итого получаем 8, и еще немного добавляет оценка процессорного времени на обработку строк.

```sql
SELECT current_setting('seq_page_cost');
-- 1
```

В строке Index Cond плана указываются только те условия, по которым происходит обращение к индексу или которые могут быть проверены на уровне индекса.

Дополнительные условия, которые можно проверить только по таблице, отображаются в отдельной строке Filter:

```sql
EXPLAIN
SELECT *
FROM bookings
WHERE book_ref = 'CDE08B'
  AND total_amount > 1000;
```

```console
Index Scan using bookings_pkey on bookings  (cost=0.43..8.45 rows=1 width=21)
  Index Cond: (book_ref = 'CDE08B'::bpchar)
  Filter: (total_amount > '1000'::numeric)
```

### Поиск по диапазону

Мы получаем данные из индекса, спускаясь от корня дерева к левому листовому узлу и проходя по списку листовых страниц. Поэтому индексное сканирование всегда возвращает данные в том порядке, в котором они хранятся в дереве индекса и который был указан при его создании:

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM bookings
WHERE book_ref > '000900'
  AND book_ref < '000939'
ORDER BY book_ref;
```

```console
Index Scan using bookings_pkey on bookings
  Index Cond: ((book_ref > '000900'::bpchar) AND (book_ref < '000939'::bpchar))
```

Тот же самый индекс может использоваться и для получения строк в обратном порядке:

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM bookings
WHERE book_ref > '000900'
  AND book_ref < '000939'
ORDER BY book_ref DESC;
```

```console
Index Scan Backward using bookings_pkey on bookings
  Index Cond: ((book_ref > '000900'::bpchar) AND (book_ref < '000939'::bpchar))
```

В этом случае мы спускаемся от корня дерева к правому листовому узлу, и проходим по списку листовых страниц в обратную сторону — Index Scan Backward. В обоих планах запросов нет узла для `ORDER BY`, поскольку значения в индексе уже отсортированы.

Обратите внимание на количество страниц (Buffers), которое потребовалось прочитать:

```sql
EXPLAIN (ANALYZE, BUFFERS, COSTS OFF, TIMING OFF, SUMMARY OFF)
SELECT *
FROM bookings
WHERE book_ref > '000900'
  AND book_ref < '000939'
ORDER BY book_ref DESC;
```

```console
Index Scan Backward using bookings_pkey on bookings (actual rows=5 loops=1)
  Index Cond: ((book_ref > '000900'::bpchar) AND (book_ref < '000939'::bpchar))
  Buffers: shared hit=4 read=1
Planning:
  Buffers: shared hit=8
```

Сравним поиск по диапазону с повторяющимся поиском отдельных значений. Получим тот же результат с помощью конструкции `IN`, и посмотрим, сколько страниц потребовалось прочитать в этом случае:

```sql
EXPLAIN (ANALYZE, BUFFERS, COSTS OFF)
SELECT *
FROM bookings
WHERE book_ref IN ('000906', '000909', '000917', '000930', '000938')
ORDER BY book_ref DESC;
```

```console
Index Scan Backward using bookings_pkey on bookings (actual time=0.039..0.060 rows=5 loops=1)
  Index Cond: (book_ref = ANY ('{000906,000909,000917,000930,000938}'::bpchar[]))
  Buffers: shared hit=24
Planning Time: 0.134 ms
Execution Time: 0.073 ms
```

Количество страниц увеличилось, поскольку в этом случае приходится спускаться от корня к каждому значению.

### Параллельное сканирование индекса

Сканирование индекса может выполняться в параллельном режиме. В качестве примера найдем общую сумму всех бронирований с номерами, меньшими 400000 (их примерно одна четверть от общего числа):

```sql
EXPLAIN
SELECT SUM(total_amount)
FROM bookings
WHERE book_ref < '400000';
```

```console
Finalize Aggregate  (cost=17498.93..17498.94 rows=1 width=32)
  ->  Gather  (cost=17498.71..17498.92 rows=2 width=32)
        Workers Planned: 2
        ->  Partial Aggregate  (cost=16498.71..16498.72 rows=1 width=32)
              ->  Parallel Index Scan using bookings_pkey on bookings  (cost=0.43..15927.38 rows=228534 width=6)
                    Index Cond: (book_ref < '400000'::bpchar)
```

Аналогичный план мы уже видели при параллельном последовательном сканировании, но в данном случае данные читаются с помощью индекса — узел Parallel Index Scan.

Полная стоимость складывается из стоимостей доступа к таблице и к индексу. Стоимость доступа к 1/4 табличных страниц (поделенных между процессами) оценивается аналогично последовательному сканированию:

```sql
SELECT ROUND(
               (relpages / 4.0) * CURRENT_SETTING('seq_page_cost') ::real +
               (reltuples / 4.0) / 2.4 * CURRENT_SETTING('cpu_tuple_cost'):: real
       )
FROM pg_class
WHERE relname = 'bookings';

-- 5571
```

Оценка стоимости индексного доступа не делится между процессами, так как индекс читается процессами последовательно, страница за страницей:

```sql
SELECT ROUND(
               (relpages / 4.0) * CURRENT_SETTING('random_page_cost'):: real +
               (reltuples / 4.0) * CURRENT_SETTING('cpu_index_tuple_cost'):: real +
               (reltuples / 4.0) * CURRENT_SETTING('cpu_operator_cost'):: real
       )
FROM pg_class
WHERE relname = 'bookings_pkey';

-- 9750
```

$5571 + 9750 = 15321$.

```sql
EXPLAIN (ANALYZE)
SELECT SUM(total_amount)
FROM bookings
WHERE book_ref < '400000';
```

```console
|Finalize Aggregate  (cost=17498.93..17498.94 rows=1 width=32) (actual time=78.440..81.465 rows=1 loops=1)                                                       |
|  ->  Gather  (cost=17498.71..17498.92 rows=2 width=32) (actual time=78.240..81.456 rows=3 loops=1)                                                             |
|        Workers Planned: 2                                                                                                                                      |
|        Workers Launched: 2                                                                                                                                     |
|        ->  Partial Aggregate  (cost=16498.71..16498.72 rows=1 width=32) (actual time=75.337..75.338 rows=1 loops=3)                                            |
|              ->  Parallel Index Scan using bookings_pkey on bookings  (cost=0.43..15927.38 rows=228534 width=6) (actual time=0.063..50.635 rows=175825 loops=3)|
|                    Index Cond: (book_ref < '400000'::bpchar)                                                                                                   |
|Planning Time: 0.150 ms                                                                                                                                         |
|Execution Time: 81.510 ms                                                                                                                                       |
```

Значением параметра `cpu_operator_cost` оценивается операция сравнения значений («меньше»).

## Число рабочих процессов

- Равно нулю (параллельный план не строится), если размер выборки < `min_parallel_index_scan_size` = 512KB
- Фиксировано, если для таблицы указан параметр хранения `parallel_workers`
- Вычисляется по формуле $1 + \lfloor log_{3}(размер \ выборки / min\_parallel\_index\_scan\_size) \rfloor$
- Но не больше, чем `max_parallel_workers_per_gather`
