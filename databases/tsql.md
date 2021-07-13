# T-SQL

## Count objects

You can use T-SQL to count number of stored procedures, views, tables or functions in a database by using the database `INFORMATION_SCHEMA` view.

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
