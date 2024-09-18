# SQL Server. T-SQL. PL/SQL

## Table of contents

- [SQL Server. T-SQL. PL/SQL](#sql-server-t-sql-plsql)
  - [Table of contents](#table-of-contents)
  - [SQL Server](#sql-server)
  - [T-SQL](#t-sql)
    - [Variable declaration](#variable-declaration)
    - [Commands](#commands)
      - [`SET NOCOUNT`](#set-nocount)
    - [Count tables in database](#count-tables-in-database)
    - [Count stored procedures in database](#count-stored-procedures-in-database)
    - [Count functions in database](#count-functions-in-database)
    - [Triggers](#triggers)
  - [PL/SQL](#plsql)

## SQL Server

[↑ **Microsoft SQL Server**](https://en.wikipedia.org/wiki/Microsoft_SQL_Server), or **SQL Server** for short, is a proprietary relational database management system developed by Microsoft.

`docker-compose.yaml` file:

```yaml
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

## T-SQL

[↑ **Transact-SQL**](https://en.wikipedia.org/wiki/Transact-SQL) or **T-SQL** is Microsoft's and Sybase's proprietary extension to the SQL used to interact with relational databases.

T-SQL expands on the SQL standard to include procedural programming, local variables, various support functions for string processing, date processing, mathematics, etc.

### Variable declaration

```sql
DECLARE @start VARCHAR(30);
SET @start = '2021-08-04';

SELECT COUNT(*)
FROM [Database].[Schema].[Table]
WHERE DateLastChanged > @start;
```

### Commands

#### `SET NOCOUNT`

Stops the message that shows the count of the number of rows affected by a T-SQL statement or stored procedure from being returned as part of the result set.

```sql
SET NOCOUNT { ON | OFF }
```

### Count tables in database

```sql
SELECT COUNT(*) AS TABLE_COUNT
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_TYPE = 'BASE TABLE'
```

Number of **views** in a database:

```sql
SELECT COUNT(*) AS VIEW_COUNT
FROM INFORMATION_SCHEMA.VIEWS
```

### Count stored procedures in database

```sql
SELECT COUNT(*) AS PROCEDURE_COUNT
FROM INFORMATION_SCHEMA.ROUTINES
WHERE ROUTINE_TYPE = 'PROCEDURE'
```

### Count functions in database

```sql
SELECT COUNT(*) AS FUNCTION_COUNT
FROM INFORMATION_SCHEMA.ROUTINES
WHERE ROUTINE_TYPE = 'FUNCTION'
```

### Triggers

```sql
CREATE TRIGGER [dbo].[TRIGGER_NAME] ON [dbo].[TABLE_NAME]
FOR INSERT, UPDATE
AS

INSERT INTO [dbo].[TABLE_NAME] (OrderNumber, DateCreated, DateLastChanged)
SELECT OrderNumber, DateCreated, GETDATE()
FROM INSERTED

GO

ALTER TABLE [dbo].[TABLE_NAME] ENABLE TRIGGER [TRIGGER_NAME]
GO
```

[↑ Loading Millions Of Rows Of Test Data In Seconds](https://www.youtube.com/watch?v=Obsn8nHdnIY).

## PL/SQL

**PL/SQL** or **Procedural Language for SQL** is Oracle Corporation's procedural extension for SQL and the [↑ Oracle Database](https://en.wikipedia.org/wiki/Oracle_Database).

| Function or operator       | Description                                                                                                                                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `\|\|`                     | Сoncatenates 2 or more strings together.<br>`'Tech on' \|\| ' the Net'`                                                                                                                            |
| `CONCAT(string1, string2)` | Concatenates two strings together.<br>`CONCAT('Tech on', ' the Net')`                                                                                                                              |
| `NVL`                      | Substitutes `Null` value with other value.<br>`SELECT NVL(supplier_city, 'n/a')`<br>`FROM suppliers;`                                                                                              |
| `ROWNUM`                   | Works like `TOP` in T-SQL.<br>`SELECT *`<br>`FROM MY_TABLE`<br>`WHERE ROWNUM <= 80;`                                                                                                               |
| `SQL % ROWCOUNT`           | Number of rows affected by an `UPDATE`<br>`DECLARE`<br>`I NUMBER;`<br>`BEGIN`<br>`UPDATE MY_TABLE`<br>`SET USR_CORRECTOR = 'Aleksei'`<br>`WHERE ID = 2739875;`<br>`I := SQL % ROWCOUNT;`<br>`END;` |
| `TO_DATE`                  | `SELECT TO_DATE('2012-06-05', 'YYYY-MM-DD')`<br>`FROM DUAL;`                                                                                                                                       |
