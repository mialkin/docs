# Isolation levels in SQL Server

## Table of contents

- [Isolation levels in SQL Server](#isolation-levels-in-sql-server)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
    - [DDL \& DML](#ddl--dml)

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
VALUES ('bob', 100, '2020-09-06 15:09:38'),
       ('alice', 100, '2020-09-06 15:09:38');
GO
```
