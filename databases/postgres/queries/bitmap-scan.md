# Bitmap scan

- Построение битовой карты (Bitmap Index Scan)
- Сканирование по битовой карте (Bitmap Heap Scan)
- Точные и неточные фрагменты
- Параллельное сканирование (Parallel Bitmap Heap Scan)
- Объединение битовых карт

## Table of contents

- [Bitmap scan](#bitmap-scan)
  - [Table of contents](#table-of-contents)
  - [Bitmap Index Scan. Bitmap Heap Scan](#bitmap-index-scan-bitmap-heap-scan)
  - [Объединение битовых карт](#объединение-битовых-карт)
  - [Parallel Bitmap Heap Scan](#parallel-bitmap-heap-scan)
  - [Неточные фрагменты](#неточные-фрагменты)

## Bitmap Index Scan. Bitmap Heap Scan

Будем рассматривать таблицу бронирований bookings.

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

Создадим на ней два дополнительных индекса:

```sql
CREATE INDEX book_date_test ON bookings (book_date);
```

```sql
CREATE INDEX total_amount_test ON bookings (total_amount);
```

Посмотрим, какой метод доступа будет выбран для поиска диапазона.

```sql
EXPLAIN
SELECT *
FROM bookings
WHERE total_amount < 10000;
```

```console
Bitmap Heap Scan on bookings  (cost=1219.51..15518.92 rows=64913 width=21)                   
  Recheck Cond: (total_amount < '10000'::numeric)                                            
  ->  Bitmap Index Scan on bookings_total_amount_idx  (cost=0.00..1203.28 rows=64913 width=0)
        Index Cond: (total_amount < '10000'::numeric)                                        
```

Выбран метод доступа Bitmap Scan. Он состоит из двух узлов:

- Bitmap Index Scan читает индекс и строит битовую карту;
- Bitmap Heap Scan читает табличные страницы, используя построенную карту.
Обратите внимание, что карта должна быть построена полностью, прежде чем ее можно будет использовать.

## Объединение битовых карт

Кроме того, что битовая карта позволяет избежать повторных чтений табличных страниц, с ее помощью можно объединять нескольких условий.

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM bookings
WHERE total_amount < 10000
   OR total_amount > 100000;
```

```console
Bitmap Heap Scan on bookings                                                             
  Recheck Cond: ((total_amount < '10000'::numeric) OR (total_amount > '100000'::numeric))
  ->  BitmapOr                                                                           
        ->  Bitmap Index Scan on total_amount_test                                       
              Index Cond: (total_amount < '10000'::numeric)                              
        ->  Bitmap Index Scan on total_amount_test                                       
              Index Cond: (total_amount > '100000'::numeric)                             
```

Здесь сначала были построены две битовые карты - по одной на каждое условие, а затем объединены побитовой операцией «или».

Таким же образом могут быть использованы и разные индексы.

```sql
EXPLAIN (COSTS OFF)
SELECT *
FROM bookings
WHERE total_amount < 10000
   OR book_date = bookings.now() - INTERVAL '1 day';
```

```console
Bitmap Heap Scan on bookings                                                                                                                 
  Recheck Cond: ((total_amount < '10000'::numeric) OR (book_date = ('2017-08-15 15:00:00+00'::timestamp with time zone - '1 day'::interval)))
  ->  BitmapOr                                                                                                                               
        ->  Bitmap Index Scan on total_amount_test                                                                                           
              Index Cond: (total_amount < '10000'::numeric)                                                                                  
        ->  Bitmap Index Scan on book_date_test                                                                                              
              Index Cond: (book_date = ('2017-08-15 15:00:00+00'::timestamp with time zone - '1 day'::interval))                             
```

## Parallel Bitmap Heap Scan

Сканирование по битовой карте может работать в параллельном режиме. Сколько бронирований сделано за последний месяц на сумму до 20 тысяч?

```sql
SELECT bookings.now() - INTERVAL '1 months' AS d;
```

```sql
EXPLAIN (COSTS OFF)
SELECT COUNT(*)
FROM bookings
WHERE total_amount < 20000
  AND book_date > '2017-07-15 15:00:00';
```

```console
Finalize Aggregate                                                                                    
  ->  Gather                                                                                          
        Workers Planned: 2                                                                            
        ->  Partial Aggregate                                                                         
              ->  Parallel Bitmap Heap Scan on bookings                                               
                    Recheck Cond: (book_date > '2017-07-15 15:00:00+00'::timestamp with time zone)    
                    Filter: (total_amount < '20000'::numeric)                                         
                    ->  Bitmap Index Scan on book_date_test                                           
                          Index Cond: (book_date > '2017-07-15 15:00:00+00'::timestamp with time zone)
```

Узел Bitmap Index Scan выполняется ведущим процессом, который строит битовую карту. Параллельно выполняется только сканирование таблицы по уже готовой битовой карте — узел Parallel Bitmap Heap Scan.

## Неточные фрагменты

Битовая карта без потери точности строится пока размер карты не превышает `work_mem`, информация хранится с точностью до версии строки.

Битовая карта с потерей точности строится если память закончилась, происходит огрубление части уже построенной карты до отдельных страниц. Требуется примерно 1 МБ памяти на 64 ГБ данных; ограничение памяти может быть превышено.

Битовая карта всегда располагается в оперативной памяти. Временные файлы на диске для битовой карты никогда не создаются.

Узел Bitmap Heap Scan показывает условие перепроверки (Recheck Cond). Сама же перепроверка выполняется не всегда, а только при потере точности, когда битовая карта не помещается в память.

Повторим запрос, изменив условие для получения бронирований до 10 тысяч:

```sql
SELECT bookings.now() - INTERVAL '1 months' AS d;
```

```sql
EXPLAIN (ANALYZE, COSTS OFF, TIMING OFF )
SELECT COUNT(*)
FROM bookings
WHERE total_amount < 10000
  AND book_date > '2017-07-15 15:00:00';
```

```console
Aggregate (actual rows=1 loops=1)                                                                                             
  ->  Bitmap Heap Scan on bookings (actual rows=5386 loops=1)                                                                 
        Recheck Cond: ((total_amount < '10000'::numeric) AND (book_date > '2017-07-15 15:00:00+00'::timestamp with time zone))
        Heap Blocks: exact=4413                                                                                               
        ->  BitmapAnd (actual rows=0 loops=1)                                                                                 
              ->  Bitmap Index Scan on total_amount_test (actual rows=63944 loops=1)                                          
                    Index Cond: (total_amount < '10000'::numeric)                                                             
              ->  Bitmap Index Scan on book_date_test (actual rows=178142 loops=1)                                            
                    Index Cond: (book_date > '2017-07-15 15:00:00+00'::timestamp with time zone)                              
Planning Time: 0.169 ms                                                                                                       
Execution Time: 26.465 ms           
```

Строка «Heap Blocks: exact» говорит о том, что все фрагменты битовой карты построены с точностью до строк — перепроверка не выполняется.

Уменьшим размер выделяемой памяти.

```sql
SET WORK_MEM = '64kB';
```

```console
Finalize Aggregate (actual rows=1 loops=1)                                                                                                
  ->  Gather (actual rows=3 loops=1)                                                                                                      
        Workers Planned: 2                                                                                                                
        Workers Launched: 2                                                                                                               
        ->  Partial Aggregate (actual rows=1 loops=3)                                                                                     
              ->  Parallel Bitmap Heap Scan on bookings (actual rows=1795 loops=3)                                                        
                    Recheck Cond: ((total_amount < '10000'::numeric) AND (book_date > '2017-07-15 15:00:00+00'::timestamp with time zone))
                    Rows Removed by Index Recheck: 648741                                                                                 
                    Heap Blocks: exact=303 lossy=4241                                                                                     
                    ->  BitmapAnd (actual rows=0 loops=1)                                                                                 
                          ->  Bitmap Index Scan on total_amount_test (actual rows=63944 loops=1)                                          
                                Index Cond: (total_amount < '10000'::numeric)                                                             
                          ->  Bitmap Index Scan on book_date_test (actual rows=178142 loops=1)                                            
                                Index Cond: (book_date > '2017-07-15 15:00:00+00'::timestamp with time zone)                              
Planning Time: 0.160 ms                                                                                                                   
Execution Time: 116.360 ms                                                                                                                
```

Здесь появились lossy-фрагменты битовой карты - с точностью до страниц. Также указано, сколько строк не прошло перепроверку условия (Rows Removed by Index Recheck).

Планировщик переключился на параллельный план, поскольку стоимость сканирования по битовой карте (теперь неточной) увеличилась.

Восстановим значение параметра.

```sql
RESET WORK_MEM;
```
