# T-SQL

**Transact-SQL** or **T-SQL** is Microsoft's and Sybase's proprietary extension to the SQL used to interact with relational databases.

T-SQL expands on the SQL standard to include procedural programming, local variables, various support functions for string processing, date processing, mathematics, etc.

## Table of contents

- [T-SQL](#t-sql)
  - [Table of contents](#table-of-contents)
  - [Variables](#variables)
    - [Variable declaration](#variable-declaration)
  - [Commands](#commands)
    - [`SET NOCOUNT`](#set-nocount)
  - [Count diffrenet database objects](#count-diffrenet-database-objects)
    - [Count tables in database](#count-tables-in-database)
    - [Count stored procedures in database](#count-stored-procedures-in-database)
    - [Count functions in database](#count-functions-in-database)
  - [Triggers](#triggers)
  - [Links](#links)

## Variables

### Variable declaration

```sql
DECLARE @start VARCHAR(30);   
SET @start = '2021-08-04';

SELECT COUNT(*)
FROM [Database].[Schema].[Table]
WHERE DateLastChanged > @start;
```

## Commands

### `SET NOCOUNT`

Stops the message that shows the count of the number of rows affected by a T-SQL statement or stored procedure from being returned as part of the result set.

```sql
SET NOCOUNT { ON | OFF }  
```

## Count diffrenet database objects

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

## Triggers

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

## Links

[↑ Loading Millions Of Rows Of Test Data In Seconds](https://www.youtube.com/watch?v=Obsn8nHdnIY).
