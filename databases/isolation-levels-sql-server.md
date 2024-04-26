# SQL Server isolation levels

## Table of contents

- [SQL Server isolation levels](#sql-server-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [DDL \& DML](#ddl--dml)
  - [Set isolation level](#set-isolation-level)
  - [Get current isolation levels](#get-current-isolation-levels)
  - [Commit \& rollback transaction](#commit--rollback-transaction)
  - [Read uncommitted](#read-uncommitted)
    - [Updating](#updating)
    - [Inserting](#inserting)
    - [Deleting](#deleting)
  - [Timeout](#timeout)
  - [Delay](#delay)

## Running

`docker-compose.yaml` file:

```yaml
version: "3.8"

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

## Read uncommitted

### Updating

```sql
-- T1
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';
```

This query will output 200 as Bob's balance:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

SELECT *
FROM simple_bank.accounts;
```

But this query will hang until you cancel it, or until you commit/rollback T1:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

SELECT *
FROM simple_bank.accounts;
```

Even a query with `WHERE name = 'Alice'` predicate will hang:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

SELECT *
FROM simple_bank.accounts
WHERE name = 'Alice';
```

Using any other serialization level up to and including `SERIALIZABLE` also does not prevent hang which is the expected behavior.

### Inserting

```sql
-- T1
BEGIN TRANSACTION;

INSERT INTO simple_bank.accounts(name, balance)
VALUES ('Alex', 100);
```

This will work and it will show inserted row:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

SELECT *
FROM simple_bank.accounts;
```

This will hang:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

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

This will work and will show that the row was deleted:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

SELECT *
FROM simple_bank.accounts;
```

This will hang:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

SELECT *
FROM simple_bank.accounts;
```

## Timeout

It's possible to specify timeout explicitly using [↑ `SET LOCK_TIMEOUT`](https://learn.microsoft.com/ru-ru/sql/t-sql/statements/set-lock-timeout-transact-sql) to avoid the hang:

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

SET LOCK_TIMEOUT 5000; -- 5 seconds

SELECT *
FROM simple_bank.accounts;
```

## Delay

It's possible to delay execution of a command inside transaction using [↑ `WAITFOR`](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/waitfor-transact-sql) keyword:

```sql
WAITFOR DELAY '00:00:05'; -- 5 seconds

SELECT *
FROM simple_bank.accounts;
```
