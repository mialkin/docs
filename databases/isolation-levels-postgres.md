# PostgreSQL isolation levels

## Table of contents

- [PostgreSQL isolation levels](#postgresql-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [DDL \& DML](#ddl--dml)
  - [Set isolation level](#set-isolation-level)
  - [Commit \& rollback transaction](#commit--rollback-transaction)
  - [Get current isolation levels](#get-current-isolation-levels)
  - [Summary table](#summary-table)
  - [Read uncommitted](#read-uncommitted)
    - [Updating](#updating)
    - [Inserting](#inserting)
    - [Deleting](#deleting)
  - [Delay](#delay)

## Running

`docker-compose.yaml` file:

```yaml
version: "3.9"

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
CREATE DATABASE isolation_levels;

CREATE SCHEMA simple_bank;

CREATE TABLE simple_bank.accounts
(
    id         integer GENERATED ALWAYS AS IDENTITY,
    name       text               NOT NULL,
    balance    bigint             NOT NULL,
    created_at date DEFAULT NOW() NOT NULL
);

CREATE TABLE simple_bank.accounts
(
    id         int IDENTITY,
    name       NVARCHAR(100)               NOT NULL,
    balance    BIGINT                      NOT NULL,
    created_at DATETIME2 DEFAULT GETDATE() NOT NULL
);

INSERT INTO simple_bank.accounts (name, balance, created_at)
VALUES ('Bob', 100, '2020-09-06 15:09:38'),
       ('Alice', 100, '2020-09-06 15:09:38');
```

## Set isolation level

```sql
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
-- READ COMMITTED
-- REPEATABLE READ
-- SNAPSHOT
-- SERIALIZABLE
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

## Get current isolation levels

```sql
SHOW TRANSACTION ISOLATION LEVEL;
```

## Summary table

| Isolation level  | Dirty read             | Nonrepeatable read | Phantom read           | Serialization anomaly |
| ---------------- | ---------------------- | ------------------ | ---------------------- | --------------------- |
| Read uncommitted | Allowed, but not in PG | Possible           | Possible               | Possible              |
| Read committed   | Not possible           | Possible           | Possible               | Possible              |
| Repeatable read  | Not possible           | Not possible       | Allowed, but not in PG | Possible              |
| Serializable     | Not possible           | Not possible       | Not possible           | Not possible          |

In PostgreSQL, you can request any of the four standard transaction isolation levels, but internally only three distinct isolation levels are implemented, i.e., PostgreSQL's read uncommitted mode behaves like read committed. This is because it is the only sensible way to map the standard isolation levels to PostgreSQL's multiversion concurrency control architecture.

The table also shows that PostgreSQL's repeatable read implementation does not allow phantom reads. This is acceptable under the SQL standard because the standard specifies which anomalies must not occur at certain isolation levels; higher guarantees are acceptable. The behavior of the available isolation levels is detailed in the following subsections.

[↑ 13.2. Transaction Isolation](https://www.postgresql.org/docs/16/transaction-iso.html).

## Read uncommitted

Based on snapshots, PostgreSQL isolation differs from the requirements specified in the standard — in fact, it is even stricter. Dirty reads are forbidden by design. Technically, you can specify the `READ UNCOMMITTED` level, but its behavior will be the same as that of `READ COMMITTED`.

### Updating

```sql
-- T1
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';
```

This will _not_ show any changes in Bob's balance:

```sql
-- T2
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

SELECT *
FROM simple_bank.accounts;
```

### Inserting

```sql
-- T1
BEGIN TRANSACTION;

INSERT INTO simple_bank.accounts(name, balance)
VALUES ('Alex', 100);
```

This will _not_ show the new inserted row:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

SELECT *
FROM simple_bank.accounts;
```

### Deleting

```sql
-- T1
BEGIN TRANSACTION;

DELETE
FROM simple_bank.accounts
WHERE name = 'Alex';
```

This will _not_ show deleted row:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

SELECT *
FROM simple_bank.accounts;
```

## Delay

It's possible to delay execution of a command inside transaction using [↑ `pg_sleep`](https://www.postgresql.org/docs/16/functions-datetime.html#FUNCTIONS-DATETIME-DELAY) function:

```sql
SELECT CURRENT_TIMESTAMP;
SELECT pg_sleep(5); -- 5 seconds
SELECT CURRENT_TIMESTAMP;
```
