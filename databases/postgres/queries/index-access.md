# Index access

- B-trees
- Index scan
- Index only scan
- Indexes with additional columns
- Duplicates in index

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
