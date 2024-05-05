# SQL Server isolation levels

## Table of contents

- [SQL Server isolation levels](#sql-server-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [DDL \& DML](#ddl--dml)
  - [Set isolation level](#set-isolation-level)
  - [Get current isolation levels](#get-current-isolation-levels)
  - [Commit \& rollback transaction](#commit--rollback-transaction)
  - [Timeout](#timeout)
  - [Delay](#delay)
  - [Read uncommitted](#read-uncommitted)
    - [`UPDATE`](#update)
    - [`INSERT`](#insert)
    - [`DELETE`](#delete)
  - [Read committed](#read-committed)
    - [`UPDATE`](#update-1)
    - [`INSERT`](#insert-1)
    - [`DELETE`](#delete-1)
  - [Repeatable read](#repeatable-read)
    - [`UPDATE`, `DELETE`](#update-delete)
    - [`INSERT`](#insert-2)
  - [Serializable](#serializable)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete)

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

## Delay

It's possible to delay execution of a command inside transaction using [↑ `WAITFOR`](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/waitfor-transact-sql) keyword:

```sql
WAITFOR DELAY '00:00:05'; -- 5 seconds

SELECT *
FROM simple_bank.accounts;
```

## Read uncommitted

### `UPDATE`

T2 outputs `200` as Bob's balance (dirty read):

```sql
-- T1
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';

WAITFOR DELAY '00:00:10'; -- 10 seconds

ROLLBACK;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

With `READ COMMITTED` T2 blocks until you cancel it, or until you commit/rollback T1:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

Using any other serialization level up to and including `SERIALIZABLE` also does not prevent T2 to block which is the expected behavior.

### `INSERT`

T2 shows inserted row, although T1 hasn't committed anything:

```sql
-- T1
BEGIN TRANSACTION;

INSERT INTO simple_bank.accounts(name, balance)
VALUES ('Alex', 100);

WAITFOR DELAY '00:00:10'; -- 10 seconds

ROLLBACK;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

This will hang and will *not* show inserted row since T1 hasn't committed:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

### `DELETE`

T2 will *not* block and will show that the row was deleted:

```sql
-- T1
BEGIN TRANSACTION;

DELETE
FROM simple_bank.accounts
WHERE name = 'Bob';

WAITFOR DELAY '00:00:10'; -- 10 seconds

ROLLBACK;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

T2 with `READ COMMITTED` isolation level will block:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

## Read committed

### `UPDATE`

T1 outputs `100` as Bob's balance at the first time and `200` the second time (non-repeatable read):

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

COMMIT;
```

To avoid non-repeatable read use `REPEATABLE READ` isolation level instead of `READ COMMITTED`. In this case T2 will block until T1 finishes. And T1 will print `100` both times.

### `INSERT`

The second `SELECT` in T1 will output a new row inserted by T2:

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

INSERT INTO simple_bank.accounts(name, balance)
VALUES ('Alex', 100);

COMMIT;
```

### `DELETE`

The second `SELECT` in T1 will see less rows because of the `DELETE` in T2:

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

DELETE
FROM simple_bank.accounts
WHERE name = 'Alex';

COMMIT;
```

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

`REPEATABLE READ` does not prevent phantom reads, so you will see different results in `SELECT`s:

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
