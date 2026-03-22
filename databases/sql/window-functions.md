# Window functions, `OVER`, `PARTITION BY`

A **window function** in SQL is a function that operates on a selected set of rows (a _window_, _partition_) and performs a calculation for that set of rows in a separate column.

## Table of contents

- [Window functions, `OVER`, `PARTITION BY`](#window-functions-over-partition-by)
  - [Table of contents](#table-of-contents)
  - [DDL \& DML](#ddl--dml)
  - [3 classes of window functions](#3-classes-of-window-functions)
    - [Aggregating](#aggregating)
    - [Ranking](#ranking)
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

## 3 classes of window functions

Window functions can be split in 3 categories:

| Aggregate | Ranking        | Value           |
| :-------- | :------------- | :-------------- |
| `AVG()`   | `ROW_NUMBER()` | `FIRST_VALUE()` |
| `COUNT()` | `RANK()`       | `LAST_VALUE()`  |
| `MAX()`   | `DENSE_RANK()` | `LAG()`         |
| `MIN()`   | `NTILE()`      | `LEAD()`        |
| `SUM()`   | `CUME_DIST()`  | `NTH_VALUE()`   |

### Aggregating

```sql
SELECT *,
       AVG(grade) OVER (PARTITION BY name),
       COUNT(*) OVER (PARTITION BY name),
       MAX(grade) OVER (PARTITION BY name),
       MIN(grade) OVER (PARTITION BY name),
       SUM(grade) OVER (PARTITION BY name)
FROM student_grades
ORDER BY name;
```

| name | subject    | grade | avg                | count | max | min | sum |
| :--- | :--------- | :---- | :----------------- | :---- | :-- | :-- | :-- |
| Маша | математика | 4     | 3.75               | 4     | 5   | 3   | 15  |
| Маша | русский    | 3     | 3.75               | 4     | 5   | 3   | 15  |
| Маша | физика     | 5     | 3.75               | 4     | 5   | 3   | 15  |
| Маша | история    | 3     | 3.75               | 4     | 5   | 3   | 15  |
| Петя | физика     | 5     | 4.3333333333333333 | 3     | 5   | 4   | 13  |
| Петя | история    | 4     | 4.3333333333333333 | 3     | 5   | 4   | 13  |
| Петя | русский    | 4     | 4.3333333333333333 | 3     | 5   | 4   | 13  |

### Ranking

```sql
SELECT *,
       ROW_NUMBER() OVER (PARTITION BY name),
       RANK() OVER (PARTITION BY name),
       DENSE_RANK() OVER (PARTITION BY name),
       NTILE(3) OVER (PARTITION BY name),
       CUME_DIST() OVER (PARTITION BY name)
FROM student_grades
ORDER BY name;
```

`ROW_NUMBER()` function returns the sequential number of a row within a partition of a result set, starting at 1 for the first row in each partition.

`NTILE(N)` function receives an integer parameter (`N`) and divides the complete set of rows into `N` subsets. Each subset has approximately the same number of rows and is identified by a number between 1 and `N`. This ID number is what `NTILE()` returns.

`RANK()` gives you the ranking within your ordered partition. Ties are assigned the same rank, with the next ranking(s) skipped. So, if you have 3 items at rank 2, the next rank listed would be ranked 5.

`DENSE_RANK()` again gives you the ranking within your ordered partition, but the ranks are consecutive. No ranks are skipped if there are ranks with multiple items. So, if you have 3 items at rank 2, the next rank listed would be ranked 3.

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
