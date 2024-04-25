# Isolation levels in SQL Server

## Table of contents

- [Isolation levels in SQL Server](#isolation-levels-in-sql-server)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [DDL \& DML](#ddl--dml)
  - [Set isolation level](#set-isolation-level)
  - [Commit \& rollback transaction](#commit--rollback-transaction)
  - [Get current isolation levels](#get-current-isolation-levels)
  - [Read uncommitted](#read-uncommitted)
  - [Add delay](#add-delay)

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
      # Use sa/yourStrong(!)Password as user/password credentials
      MSSQL_SA_PASSWORD: yourStrong(!)Password
```

### DDL & DML

```sql
CREATE DATABASE isolation_levels
GO

USE isolation_levels;
GO

CREATE SCHEMA simple_bank
GO

CREATE TABLE simple_bank.accounts
(
    id         int IDENTITY,
    name       NVARCHAR(100)               NOT NULL,
    balance    BIGINT                      NOT NULL,
    created_at DATETIME2 DEFAULT GETDATE() NOT NULL
)
GO

INSERT INTO simple_bank.accounts (name, balance, created_at)
VALUES ('Bob', 100, '2020-09-06 15:09:38'),
       ('Alice', 100, '2020-09-06 15:09:38');
GO
```

## Set isolation level

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
-- READ COMMITTED
-- REPEATABLE READ
-- SNAPSHOT
-- SERIALIZABLE
```

## Commit & rollback transaction

```sql
BEGIN TRANSACTION;

COMMIT TRANSACTION;
```

```sql
BEGIN TRANSACTION;

ROLLBACK TRANSACTION;
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

## Read uncommitted

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

Even a query with following predicate will hang:

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

SELECT *
FROM simple_bank.accounts
WHERE name = 'Alice';
```

It's possible to specify timeout explicitly to avoid the hang:

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

SET LOCK_TIMEOUT 5000; -- 5 seconds

SELECT *
FROM simple_bank.accounts;
```

Using any other serialization level up to `SERIALIZABLE` also does not prevent hang which is the expected behavior.

## Add delay

It's possible to delay execution of a command inside transaction using [↑ `WAITFOR`](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/waitfor-transact-sql) keyword:

```sql
WAITFOR DELAY '00:00:05'; -- 5 seconds

SELECT *
FROM simple_bank.accounts;
```
