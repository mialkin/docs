# T-SQL

- [T-SQL](#t-sql)
  - [Variables](#variables)
  - [Commands](#commands)
    - [`SET NOCOUNT`](#set-nocount)
  - [Count](#count)
    - [Count tables in database](#count-tables-in-database)
    - [Count stored procedures in database](#count-stored-procedures-in-database)
    - [Count functions in database](#count-functions-in-database)
  - [Triggers](#triggers)

## Variables

Declare variable:

```sql
DECLARE @start VARCHAR(30);   
SET @start = '2021-08-04';

SELECT COUNT(*)
FROM [Database].[Schema].[Table]
WHERE DateLastChanged > @start;
```

## Commands

### `SET NOCOUNT`

Stops the message that shows the count of the number of rows affected by a Transact-SQL statement or stored procedure from being returned as part of the result set.

```sql
SET NOCOUNT { ON | OFF }  
```

## Count

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
