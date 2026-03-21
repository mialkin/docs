# Window functions, `OVER`, `PARTITION BY`

A **window function** in SQL is a function that operates on a selected set of rows (a _window_, _partition_) and performs a calculation for that set of rows in a separate column.

## Table of contents

- [Window functions, `OVER`, `PARTITION BY`](#window-functions-over-partition-by)
  - [Table of contents](#table-of-contents)
  - [DDL \& DML](#ddl--dml)
  - [`ORDER BY`](#order-by)

## DDL & DML

DDL:

```sql
CREATE TABLE student_grades
(
    name    varchar,
    subject varchar,
    grade   int
);
```

DML:

```sql
INSERT INTO student_grades (VALUES ('Петя', 'русский', 4),
                                   ('Петя', 'физика', 5),
                                   ('Петя', 'история', 4),
                                   ('Маша', 'математика', 4),
                                   ('Маша', 'русский', 3),
                                   ('Маша', 'физика', 5),
                                   ('Маша', 'история', 3));
```

```sql
SELECT *
FROM student_grades
ORDER BY name ASC, grade DESC;
```

| name | subject    | grade |
| :--- | :--------- | :---- |
| Маша | физика     | 5     |
| Маша | математика | 4     |
| Маша | русский    | 3     |
| Маша | история    | 3     |
| Петя | физика     | 5     |
| Петя | история    | 4     |
| Петя | русский    | 4     |

## `ORDER BY`

While `ORDER BY grade` orders rows within a _window_, the `ORDER BY name` orders rows within entire _dataset_:

```sql
SELECT *,
       ROW_NUMBER() OVER (PARTITION BY name ORDER BY grade) AS row_number
FROM student_grades
ORDER BY name;
```

| name | subject    | grade | row_number |
| :--- | :--------- | :---- | :--------- |
| Маша | русский    | 3     | 1          |
| Маша | история    | 3     | 2          |
| Маша | математика | 4     | 3          |
| Маша | физика     | 5     | 4          |
| Петя | русский    | 4     | 1          |
| Петя | история    | 4     | 2          |
| Петя | физика     | 5     | 3          |
