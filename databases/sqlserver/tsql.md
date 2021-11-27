# T-SQL

- [T-SQL](#t-sql)
  - [Commands](#commands)
    - [Declare a variable](#declare-a-variable)
    - [`SET NOCOUNT` command](#set-nocount-command)
  - [Count objects](#count-objects)
  - [Triggers](#triggers)
  - [Indexes](#indexes)
    - [Index scan](#index-scan)
    - [Index seek](#index-seek)
    - [Indexes with included columns](#indexes-with-included-columns)

## Commands

### Declare a variable

```sql
DECLARE @start VARCHAR(30);   
SET @start = '2021-08-04';

SELECT COUNT(*)
FROM [Database].[Schema].[Table]
WHERE DateLastChanged > @start;
```

### `SET NOCOUNT` command

Stops the message that shows the count of the number of rows affected by a Transact-SQL statement or stored procedure from being returned as part of the result set.

```sql
SET NOCOUNT { ON | OFF }  
```

## Count objects

Number of **tables** in a database:

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

Number of **stored procedures** in a database:

```sql
SELECT COUNT(*) AS PROCEDURE_COUNT
FROM INFORMATION_SCHEMA.ROUTINES
WHERE ROUTINE_TYPE = 'PROCEDURE'
```

Number of **functions** in a database:

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

## Indexes

### Index scan

Since a scan touches every row in the table, whether or not it qualifies, the cost is proportional to the total number of rows in the table. Thus, a scan is an efficient strategy if the table is small or if most of the rows qualify for the predicate.

### Index seek

Index seek traverses a B-tree and walks through leaf nodes seeking only the matching or qualifying rows based on the filter criteria.

### Indexes with included columns

An index with included columns can greatly improve query performance because all columns in the query are included in the index; The query optimizer can locate all columns values within the index without accessing table or clustered index resulting in fewer disk I/O operations.

> When an index contains all the columns referenced by a query, the index is typically referred to as *covering the query*.
