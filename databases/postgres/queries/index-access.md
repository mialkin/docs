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