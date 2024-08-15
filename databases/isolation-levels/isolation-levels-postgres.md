# PostgreSQL isolation levels

In PostgreSQL, you can request any of the four standard transaction isolation levels, but internally only three distinct isolation levels are implemented, i.e., PostgreSQL's read uncommitted mode behaves like read committed. This is because it is the only sensible way to map the standard isolation levels to PostgreSQL's multiversion concurrency control architecture.

The table also shows that PostgreSQL's repeatable read implementation does not allow phantom reads. This is acceptable under the SQL standard because the standard specifies which anomalies must not occur at certain isolation levels; higher guarantees are acceptable. The behavior of the available isolation levels is detailed in the following subsections.

| Isolation level  | Dirty read             | Non-repeatable read | Phantom read           | Serialization anomaly |
| ---------------- | ---------------------- | ------------------- | ---------------------- | --------------------- |
| Read uncommitted | Allowed, but not in PG | Possible            | Possible               | Possible              |
| Read committed   | Not possible           | Possible            | Possible               | Possible              |
| Repeatable read  | Not possible           | Not possible        | Allowed, but not in PG | Possible              |
| Serializable     | Not possible           | Not possible        | Not possible           | Not possible          |

[↑ 13.2. Transaction Isolation](https://www.postgresql.org/docs/16/transaction-iso.html).

## Table of contents

- [PostgreSQL isolation levels](#postgresql-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [DDL \& DML](#ddl--dml)
  - [Set isolation level](#set-isolation-level)
  - [Get current isolation levels](#get-current-isolation-levels)
  - [Commit \& rollback transaction](#commit--rollback-transaction)
  - [Delay](#delay)
  - [Read uncommitted](#read-uncommitted)
    - [Dirty read](#dirty-read)
  - [Read committed](#read-committed)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete)
  - [Repeatable read](#repeatable-read)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete-1)

## Running

`docker-compose.yaml` file:

```yaml
services:
  postgres:
    image: postgres:16.2
    container_name: isolation-levels-postgres
    ports:
      - "3500:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
```

### DDL & DML

```sql
BEGIN TRANSACTION;

CREATE TABLE accounts
(
    name    text   NOT NULL UNIQUE,
    balance bigint NOT NULL
);

INSERT INTO accounts (name, balance)
VALUES ('Bob', 100),
       ('Alice', 100);

COMMIT;
```

## Set isolation level

```sql
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
-- READ COMMITTED
-- REPEATABLE READ
-- SERIALIZABLE
```

## Get current isolation levels

```sql
SHOW TRANSACTION ISOLATION LEVEL;
```

## Commit & rollback transaction

```sql
BEGIN TRANSACTION;

COMMIT;
```

```sql
BEGIN TRANSACTION;

ROLLBACK;
```

## Delay

It's possible to delay execution of a command inside transaction using [↑ `pg_sleep`](https://www.postgresql.org/docs/16/functions-datetime.html#FUNCTIONS-DATETIME-DELAY) function:

```sql
SELECT CURRENT_TIMESTAMP;
SELECT pg_sleep(10); -- 10 seconds
SELECT CURRENT_TIMESTAMP;
```

## Read uncommitted

### Dirty read

Dirty reads are forbidden by design.

Based on snapshots, PostgreSQL isolation differs from the requirements specified in the standard — in fact, it is even stricter. Technically, you can specify the `READ UNCOMMITTED` level, but its behavior will be the same as that of `READ COMMITTED`.

```sql
-- T1
BEGIN TRANSACTION;

UPDATE accounts
SET balance = 200
WHERE name = 'Bob';

SELECT PG_SLEEP(10); -- 10 seconds

ROLLBACK; -- Or COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

SELECT *
FROM accounts
WHERE name = 'Bob';

COMMIT;
```

| name | balance |
| :--- | :------ |
| Bob  | 100     |

## Read committed

Dirty reads are impossible in Postgres, although SQL standard allows them.

### `UPDATE`, `INSERT`, `DELETE`

On `UPDATE` T1 shows different values for Bob's balance — non-repeatable read.

On `INSERT` and `DELETE` T1 shows different number of rows — phantom read.

```sql
-- T1
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;

SELECT *
FROM simple_bank.accounts;

SELECT pg_sleep(10); -- 10 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';

-- INSERT INTO simple_bank.accounts(name, balance)
-- VALUES ('Alex', 100);

-- DELETE
-- FROM simple_bank.accounts
-- WHERE name = 'Alex';

COMMIT;
```

On `UPDATE` T2 blocks until T1 commits. The final balance of Bob is `300`:

```sql
-- T1
BEGIN TRANSACTION;

UPDATE accounts
SET balance = balance + 100
WHERE name = 'Bob';

SELECT PG_SLEEP(10);

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;

UPDATE accounts
SET balance = balance + 100
WHERE name = 'Bob';

COMMIT;
```

By replacing both conditions, or by replacing just T2's condition,:

```sql
WHERE name = 'Bob';
```

with:

```sql
WHERE name = 'Bob'
  AND balance = 100;
```

we will get `200` as Bob's balance when both transactions commit.

## Repeatable read

### `UPDATE`, `INSERT`, `DELETE`

On `UPDATE` `REPEATABLE READ` prevents non-repeatable read — T1 reads `100` as Bob's balance both times.

On `INSERT` and `DELETE` `REPEATABLE READ` prevents phantom reads — T1 reads both times the same number of rows.

```sql
-- T1
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;

SELECT *
FROM simple_bank.accounts;

SELECT pg_sleep(10); -- 10 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';

-- INSERT INTO simple_bank.accounts(name, balance)
-- VALUES ('Alex', 100);

-- DELETE
-- FROM simple_bank.accounts
-- WHERE name = 'Alex';

COMMIT;
```
