# Index access

## Table of contents

- [Index access](#index-access)
  - [Table of contents](#table-of-contents)
  - [Index scan](#index-scan)
    - [Поиск по диапазону](#поиск-по-диапазону)
    - [Параллельное сканирование индекса](#параллельное-сканирование-индекса)
    - [Число рабочих процессов](#число-рабочих-процессов)
  - [Index only scan](#index-only-scan)
  - [Include-индексы](#include-индексы)
  - [Parallel index only scan](#parallel-index-only-scan)
  - [Дубликат в индексе](#дубликат-в-индексе)

## Index scan

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

### Число рабочих процессов

- Равно нулю (параллельный план не строится), если размер выборки < `min_parallel_index_scan_size` = 512KB
- Фиксировано, если для таблицы указан параметр хранения `parallel_workers`
- Вычисляется по формуле $1 + \lfloor log_{3}(размер \ выборки / min\_parallel\_index\_scan\_size) \rfloor$
- Но не больше, чем `max_parallel_workers_per_gather`

Установим максимальное число параллельно работающих процессов, которые могут запускаться одним узлом Gather, в значение четыре:

```sql
SET max_parallel_workers_per_gather = 4;
```

Посмотрим значение конфигурационного параметра `min_parallel_index_scan_size`:

```sql
SHOW min_parallel_index_scan_size;
-- 512kB
```

И выполним следующий запрос:

```sql
EXPLAIN
SELECT SUM(total_amount)
FROM bookings
WHERE book_ref < '400000';
```

```console
Finalize Aggregate  (cost=16853.98..16853.99 rows=1 width=32)
  ->  Gather  (cost=16853.66..16853.97 rows=3 width=32)
        Workers Planned: 3
        ->  Partial Aggregate  (cost=15853.66..15853.67 rows=1 width=32)
              ->  Parallel Index Scan using bookings_pkey on bookings  (cost=0.43..15411.33 rows=176929 width=6)
                    Index Cond: (book_ref < '400000'::bpchar)
```

Было запланировано три параллельных процесса. Если увеличить значение `min_parallel_index_scan_size` до 10 мегабайт, планировщик запланирует лишь один процесс:

```sql
SET min_parallel_index_scan_size = '10MB';
```

И повторим запрос:

```console
Finalize Aggregate  (cost=18675.11..18675.12 rows=1 width=32)
  ->  Gather  (cost=18674.99..18675.10 rows=1 width=32)
        Workers Planned: 1
        ->  Partial Aggregate  (cost=17674.99..17675.00 rows=1 width=32)
              ->  Parallel Index Scan using bookings_pkey on bookings  (cost=0.43..16868.40 rows=322636 width=6)
                    Index Cond: (book_ref < '400000'::bpchar)
```

```sql
RESET min_parallel_index_scan_size;
```

## Index only scan

Если вся необходимая информация содержится в самом индексе, то нет необходимости обращаться к таблице - за исключением проверки видимости:

```sql
EXPLAIN
SELECT book_ref
FROM bookings
WHERE book_ref <= '100000';
```

```console
Index Only Scan using bookings_pkey on bookings  (cost=0.43..4208.82 rows=147565 width=7)
  Index Cond: (book_ref <= '100000'::bpchar)
```

Посмотрим план этого запроса с помощью EXPLAIN ANALYZE:

```sql
EXPLAIN (ANALYZE, COSTS OFF, TIMING OFF, SUMMARY OFF)
SELECT book_ref
FROM bookings
WHERE book_ref <= '100000';
```

```console
Index Only Scan using bookings_pkey on bookings (actual rows=132109 loops=1)
  Index Cond: (book_ref <= '100000'::bpchar)
  Heap Fetches: 0
```

Строка Heap Fetches показывает, сколько версий строк было проверено с помощью таблицы. В данном случае карта видимости содержит актуальную информацию, обращаться к таблице не потребовалось.

Обновим первую строку таблицы:

```sql
UPDATE bookings
SET total_amount = total_amount
WHERE book_ref = '000004';
```

```sql
EXPLAIN (ANALYZE, COSTS OFF, TIMING OFF, SUMMARY OFF)
SELECT book_ref
FROM bookings
WHERE book_ref <= '100000';
```

```console
Index Only Scan using bookings_pkey on bookings (actual rows=132109 loops=1)
  Index Cond: (book_ref <= '100000'::bpchar)
  Heap Fetches: 157
```

Проверять приходится все версии, попадающие на измененную страницу.

## Include-индексы

Рассмотрим таблицу билетов:

```sql
psql -U postgres
\c demo
\d tickets
```

```console
                        Table "bookings.tickets"
     Column     |         Type          | Collation | Nullable | Default
----------------+-----------------------+-----------+----------+---------
 ticket_no      | character(13)         |           | not null |
 book_ref       | character(6)          |           | not null |
 passenger_id   | character varying(20) |           | not null |
 passenger_name | text                  |           | not null |
 contact_data   | jsonb                 |           |          |
Indexes:
    "tickets_pkey" PRIMARY KEY, btree (ticket_no)
Foreign-key constraints:
    "tickets_book_ref_fkey" FOREIGN KEY (book_ref) REFERENCES bookings(book_ref)
Referenced by:
    TABLE "ticket_flights" CONSTRAINT "ticket_flights_ticket_no_fkey" FOREIGN KEY (ticket_no) REFERENCES tickets(ticket_no)
```

Индекс tickets_pkey не является покрывающим для приведенного запроса, поскольку в нем требуется не только столбец `ticket_no` (есть в индексе), но и `book_ref` :

```sql
EXPLAIN (ANALYZE, BUFFERS, COSTS OFF, SUMMARY OFF)
SELECT ticket_no, book_ref
FROM tickets
WHERE ticket_no > '0005435990286';
```

```console
Index Scan using tickets_pkey on tickets (actual time=2.088..12.958 rows=7146 loops=1)
  Index Cond: (ticket_no > '0005435990286'::bpchar)
  Buffers: shared hit=74 read=153
Planning:
  Buffers: shared hit=65 read=5
```

В данном случае было прочитано 227 страниц (Buffers).

Создадим include-индекс, добавив в него неключевой столбец book_ref, так как он требуется запросу:

```sql
CREATE UNIQUE INDEX ON tickets (ticket_no) INCLUDE (book_ref);
```

Повторим запрос:

```console
Index Only Scan using tickets_ticket_no_book_ref_idx on tickets (actual time=0.094..3.946 rows=7146 loops=1)
  Index Cond: (ticket_no > '0005435990286'::bpchar)
  Heap Fetches: 0
  Buffers: shared hit=4 read=35
Planning:
  Buffers: shared hit=19 read=4
```

Теперь оптимизатор выбирает метод Index Only Scan и использует только что созданный индекс. Количество прочитанных страниц сократилось. Поскольку карта видимости актуальна, обращаться к таблице не пришлось (Heap Fetches: 0) .

B include-индекс можно включать столбцы с типами данных, которые не поддерживаются В-деревом (например, геометрические типы и XML).

## Parallel index only scan

Сканирование только индекса также может выполняться параллельно:

```sql
EXPLAIN
SELECT COUNT(book_ref)
FROM bookings
WHERE book_ref <= '400000';
```

```console
Finalize Aggregate  (cost=14004.94..14004.95 rows=1 width=8)
  ->  Gather  (cost=14004.72..14004.93 rows=2 width=8)
        Workers Planned: 2
        ->  Partial Aggregate  (cost=13004.72..13004.73 rows=1 width=8)
              ->  Parallel Index Only Scan using bookings_pkey on bookings  (cost=0.43..12433.39 rows=228534 width=7)
                    Index Cond: (book_ref <= '400000'::bpchar)
```

Стоимость доступа к таблице здесь учитывает только обработку строк, без ввода-вывода:

```sql
SELECT ROUND(
               (reltuples / 4.0) / 2.4 * CURRENT_SETTING('cpu_tuple_cost')::real
       )
FROM pg_class
WHERE relname = 'bookings';

-- 2199
```

## Дубликат в индексе

Сравним размер индекса без исключения дубликатов и с исключением. Пример подобран таким образом, что в индексе должно быть много повторяющихся значений.

Сначала создадим индекс, отключив исключение дубликатов с помощью параметра хранения `deduplicate_items`:

```sql
CREATE INDEX dedup_test ON ticket_flights (fare_conditions)
    WITH (deduplicate_items = off);
```

Посмотрим размер созданного индекса:

```sql
SELECT PG_SIZE_PRETTY(PG_TOTAL_RELATION_SIZE('dedup_test'));
-- 187 MB
```

Выполним запрос с помощью индекса, временно отключив для этого последовательное сканирование.
Обратите внимание на значение Buffers и время выполнения запроса.

```sql
SET enable_seqscan = off;
```

```sql
EXPLAIN (ANALYZE, BUFFERS, COSTS OFF)
SELECT fare_conditions
FROM ticket_flights;
```

```console
Index Only Scan using dedup_test on ticket_flights (actual time=0.042..840.232 rows=8391852 loops=1)
  Heap Fetches: 0
  Buffers: shared hit=5 read=23876 written=1
Planning:
  Buffers: shared hit=35 read=1
Planning Time: 0.403 ms
Execution Time: 1160.329 ms
```

Удалим индекс и создадим его заново. Параметр хранения `deduplicate_items` He указываем, так как по умолчанию он включен:

```sql
DROP INDEX dedup_test;
```

```sql
CREATE INDEX dedup_test ON ticket_flights (fare_conditions);
```

Опять посмотрим размер индекса:

```sql
SELECT PG_SIZE_PRETTY(PG_TOTAL_RELATION_SIZE('dedup_test'));
-- 56 MB
```

Размер сократился более чем в три раза.

Повторно выполним запрос:

```sql
EXPLAIN (ANALYZE, BUFFERS, COSTS OFF)
SELECT fare_conditions
FROM ticket_flights;
```

```console
Index Only Scan using dedup_test on ticket_flights (actual time=0.032..628.219 rows=8391852 loops=1)
  Heap Fetches: 0
  Buffers: shared hit=5 read=7077
Planning:
  Buffers: shared hit=5 read=1 dirtied=1
Planning Time: 0.211 ms
Execution Time: 969.614 ms
```

Количество Buffers тоже сократилось примерно в три раза. И запрос стал выполняться быстрее.

```sql
RESET enable_seqscan;
```
