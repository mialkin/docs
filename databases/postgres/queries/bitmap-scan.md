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
