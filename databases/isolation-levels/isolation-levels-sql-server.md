# SQL Server isolation levels

## Table of contents

- [SQL Server isolation levels](#sql-server-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [DDL \& DML](#ddl--dml)
  - [Set isolation level](#set-isolation-level)
  - [Get current isolation levels](#get-current-isolation-levels)
  - [Commit \& rollback transaction](#commit--rollback-transaction)
  - [Delay](#delay)
  - [Timeout](#timeout)
  - [Read uncommitted](#read-uncommitted)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete)
  - [Read committed](#read-committed)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete-1)
  - [Repeatable read](#repeatable-read)
    - [`UPDATE`, `DELETE`](#update-delete)
    - [`INSERT`](#insert)
  - [Serializable](#serializable)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete-2)

## Running

`docker-compose.yaml` file:

```yaml
version: "3.9"

services:
  sql-server:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: isolation-levels-sql-server
    ports:
      - "3400:1433"
    environment:
      ACCEPT_EULA: Y
      MSSQL_SA_PASSWORD: yourStrong(!)Password
```

### DDL & DML

```sql
CREATE DATABASE isolation_levels;
GO

USE isolation_levels;
GO

CREATE SCHEMA simple_bank;
GO

CREATE TABLE simple_bank.accounts
(
    id         int IDENTITY,
    name       NVARCHAR(100)               NOT NULL,
    balance    BIGINT                      NOT NULL,
    created_at DATETIME2 DEFAULT GETDATE() NOT NULL
);
GO

INSERT INTO simple_bank.accounts (name, balance, created_at)
VALUES ('Bob', 100, '2020-09-06 15:09:38'),
       ('Alice', 100, '2020-09-06 15:09:38');
GO
```

## Set isolation level

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
-- READ COMMITTED
-- REPEATABLE READ
-- SNAPSHOT
-- SERIALIZABLE
```

## Get current isolation levels

```sql
CREATE FUNCTION simple_bank.cil()
    RETURNS CHAR(20)
BEGIN
    DECLARE @cil CHAR(20);
    SET @cil = (SELECT CASE transaction_isolation_level
                           WHEN 0 THEN 'Unspecified'
                           WHEN 1 THEN 'ReadUncommitted'
                           WHEN 2 THEN 'ReadCommitted'
                           WHEN 3 THEN 'Repeatable'
                           WHEN 4 THEN 'Serializable'
                           WHEN 5 THEN 'Snapshot' END AS transaction_isolation_level
                FROM sys.dm_exec_sessions
                WHERE session_id = @@SPID);
    RETURN @cil
END

SELECT simple_bank.cil();
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

It's possible to delay execution of a command inside transaction using [↑ `WAITFOR`](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/waitfor-transact-sql) keyword:

```sql
WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM simple_bank.accounts;
```

## Timeout

It's possible to specify timeout explicitly using [↑ `SET LOCK_TIMEOUT`](https://learn.microsoft.com/ru-ru/sql/t-sql/statements/set-lock-timeout-transact-sql) to avoid the hang:

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SET LOCK_TIMEOUT 5000; -- 5 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

## Read uncommitted

### `UPDATE`, `INSERT`, `DELETE`

On `UPDATE` T2 outputs `200` as Bob's balance — dirty read.

On `INSERT` and `DELETE` T2 outputs different number of rows — also dirty read.

To avoid dirty read use `READ COMMITTED` for T2. In this case T2 blocks until you commit/rollback T1 or until you cancel T2.

```sql
-- T1
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';

-- INSERT INTO simple_bank.accounts(name, balance)
-- VALUES ('Alex', 100);

-- DELETE
-- FROM simple_bank.accounts
-- WHERE name = 'Alex';

WAITFOR DELAY '00:00:10'; -- 10 seconds

ROLLBACK; -- Or COMMIT;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

## Read committed

### `UPDATE`, `INSERT`, `DELETE`

On `UPDATE` T1 outputs `100` as Bob's balance at the first time and `200` the second time — non-repeatable read.

On `INSERT` and `DELETE` T1 sees different number of rows — phantom read.

To avoid non-repeatable read use `REPEATABLE READ` isolation level for T1. In this case T2 will block until T1 finishes. And T1 will print `100` both times.

To avoid phantom read on `DELETE` use `REPEATABLE READ` isolation level for T1.

To avoid phantom read on `INSERT` use `SERIALIZABLE` isolation level for T1.

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

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

On `UPDATE` T2 commits without blocking until T1 commits. After T1 commits the final balance of Bob is `200`:

```sql
-- T1
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = balance + 100
WHERE name = 'Bob';

WAITFOR DELAY '00:00:10'; -- 10 seconds

COMMIT;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

UPDATE simple_bank.accounts
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

we get the same `200` as Bob's balance.

## Repeatable read

### `UPDATE`, `DELETE`

T1 outputs the same result both times while T2 blocks until T1 commits:

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

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

-- DELETE
-- FROM simple_bank.accounts
-- WHERE name = 'Alex';

COMMIT;
```

### `INSERT`

On `INSERT` `REPEATABLE READ` does not prevent phantom read, so you will see different results in two `SELECT`s:

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION;

INSERT INTO simple_bank.accounts(name, balance)
VALUES ('Alex', 100);

COMMIT;
```

## Serializable

### `UPDATE`, `INSERT`, `DELETE`

Both `SELECT`s in T1 will the same result sets  for `UPDATE`, `INSERT` and `DELETE`. T2 blocks until T1 commits:

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

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
