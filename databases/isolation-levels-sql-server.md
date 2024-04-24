# Isolation levels in SQL Server

## Table of contents

- [Isolation levels in SQL Server](#isolation-levels-in-sql-server)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
  - [DDL](#ddl)

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

## DDL

```sql
CREATE DATABASE isolation_levels
GO

USE isolation_levels;
GO

CREATE SCHEMA simple_bank
GO
```
