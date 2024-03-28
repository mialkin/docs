# Postgres isolation levels

```sql
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- DO WORK

COMMIT;

ROLLBACK;
```

Other available isolation levels: `REPEATABLE READ`, `SERIALIZABLE`.

Based on snapshots, PostgreSQL isolation differs from the requirements specified in the standard — in fact, it is even stricter. Dirty reads are forbidden by design. Technically, you can specify the `READ UNCOMMITTED` level, but its behavior will be the same as that of `READ COMMITTED`.

## Prerequisites

Create table:

```sql
DROP TABLE IF EXISTS books;

CREATE TABLE IF NOT EXISTS books
(
    id    bigserial,
    title varchar
);
```

Insert values:

```sql
INSERT INTO books(title)
VALUES ('War and Peace');

INSERT INTO books(title)
VALUES ('Anna Karenina');
```
