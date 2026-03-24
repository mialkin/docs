# Window functions, `OVER`, `PARTITION BY`, running total, moving average

A **window function** in SQL is a function that operates on a selected set of rows (a _window_, _partition_) and performs a calculation for that set of rows in a separate column.

For a function (like `SUM()`, `RANK()`, etc) to actually behave as a "window function," it must be followed by the `OVER` clause.

A window function performs a calculation across a set of table rows that are somehow related to the current row. This is comparable to the type of calculation that can be done with an aggregate function. However, window functions do not cause rows to become grouped into a single output row like non-window aggregate calls would. Instead, the rows retain their separate identities. Behind the scenes, the window function is able to access more than just the current row of the query result.

## Table of contents

- [Window functions, `OVER`, `PARTITION BY`, running total, moving average](#window-functions-over-partition-by-running-total-moving-average)
  - [Table of contents](#table-of-contents)
  - [DDL \& DML](#ddl--dml)
  - [3 classes of window functions](#3-classes-of-window-functions)
    - [Aggregating](#aggregating)
    - [Ranking](#ranking)
  - [`ORDER BY`](#order-by)
  - [Running total](#running-total)
  - [Moving average](#moving-average)

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
INSERT INTO student_grades (VALUES ('Bob', 'geography', 4),
                                   ('Bob', 'physics', 5),
                                   ('Bob', 'history', 4),
                                   ('Anna', 'math', 4),
                                   ('Anna', 'geography', 3),
                                   ('Anna', 'physics', 5),
                                   ('Anna', 'history', 3));
```

```sql
SELECT *
FROM student_grades
ORDER BY name ASC, grade DESC;
```

| name | subject   | grade |
| :--- | :-------- | :---- |
| Anna | physics   | 5     |
| Anna | math      | 4     |
| Anna | geography | 3     |
| Anna | history   | 3     |
| Bob  | physics   | 5     |
| Bob  | history   | 4     |
| Bob  | geography | 4     |

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

| name | subject   | grade | avg                | count | max | min | sum |
| :--- | :-------- | :---- | :----------------- | :---- | :-- | :-- | :-- |
| Anna | math      | 4     | 3.75               | 4     | 5   | 3   | 15  |
| Anna | geography | 3     | 3.75               | 4     | 5   | 3   | 15  |
| Anna | physics   | 5     | 3.75               | 4     | 5   | 3   | 15  |
| Anna | history   | 3     | 3.75               | 4     | 5   | 3   | 15  |
| Bob  | physics   | 5     | 4.3333333333333333 | 3     | 5   | 4   | 13  |
| Bob  | history   | 4     | 4.3333333333333333 | 3     | 5   | 4   | 13  |
| Bob  | geography | 4     | 4.3333333333333333 | 3     | 5   | 4   | 13  |

### Ranking

```sql
SELECT *,
       ROW_NUMBER() OVER (PARTITION BY name),
       RANK() OVER (PARTITION BY name ORDER BY grade),
       DENSE_RANK() OVER (PARTITION BY name ORDER BY grade),
       NTILE(3) OVER (PARTITION BY name),
       CUME_DIST() OVER (PARTITION BY name ORDER BY grade)
FROM student_grades
ORDER BY name;
```

| name | subject   | grade | row_number | rank | dense_rank | ntile | cume_dist          |
| :--- | :-------- | :---- | :--------- | :--- | :--------- | :---- | :----------------- |
| Anna | geography | 3     | 1          | 1    | 1          | 1     | 0.5                |
| Anna | history   | 3     | 2          | 1    | 1          | 1     | 0.5                |
| Anna | math      | 4     | 3          | 3    | 2          | 2     | 0.75               |
| Anna | physics   | 5     | 4          | 4    | 3          | 3     | 1                  |
| Bob  | geography | 4     | 1          | 1    | 1          | 1     | 0.6666666666666666 |
| Bob  | history   | 4     | 2          | 1    | 1          | 2     | 0.6666666666666666 |
| Bob  | physics   | 5     | 3          | 3    | 2          | 3     | 1                  |

`ROW_NUMBER()` function returns the sequential number of a row within a partition of a result set, starting at 1 for the first row in each partition.

`NTILE(N)` function receives an integer parameter (`N`) and divides the complete set of rows into `N` subsets. Each subset has approximately the same number of rows and is identified by a number between 1 and `N`. This ID number is what `NTILE()` returns.

`RANK()` function gives you the ranking within your ordered partition. Ties are assigned the same rank, with the next ranking(s) skipped. So, if you have 3 items at rank 2, the next rank listed would be ranked 5.

`DENSE_RANK()` function gives you the ranking within your ordered partition, but the ranks are consecutive. No ranks are skipped if there are ranks with multiple items. So, if you have 3 items at rank 2, the next rank listed would be ranked 3.

`CUME_DIST()` function calculates the cumulative distribution of a value within a group of values.

Expert Tip: While `CUME_DIST()` is listed under Ranking, some documentation might group it under "Distribution Functions" along with `PERCENT_RANK()`. However, placing it under Ranking is still contextually correct for most SQL learners.

By the way, the expression with empty `OVER` also works:

```sql
SELECT *,
       ROW_NUMBER() OVER (),
       RANK() OVER (),
       DENSE_RANK() OVER (),
       NTILE(3) OVER (),
       CUME_DIST() OVER ()
FROM student_grades
ORDER BY name;
```

| name | subject   | grade | row_number | rank | dense_rank | ntile | cume_dist |
| :--- | :-------- | :---- | :--------- | :--- | :--------- | :---- | :-------- |
| Anna | math      | 4     | 4          | 1    | 1          | 2     | 1         |
| Anna | geography | 3     | 5          | 1    | 1          | 2     | 1         |
| Anna | physics   | 5     | 6          | 1    | 1          | 3     | 1         |
| Anna | history   | 3     | 7          | 1    | 1          | 3     | 1         |
| Bob  | physics   | 5     | 2          | 1    | 1          | 1     | 1         |
| Bob  | history   | 4     | 3          | 1    | 1          | 1     | 1         |
| Bob  | geography | 4     | 1          | 1    | 1          | 1     | 1         |

## `ORDER BY`

While `ORDER BY grade` orders rows within a _window_, the `ORDER BY name` orders rows within entire _dataset_:

```sql
SELECT *,
       ROW_NUMBER() OVER (PARTITION BY name ORDER BY grade) AS row_number
FROM student_grades
ORDER BY name;
```

| name | subject   | grade | row_number |
| :--- | :-------- | :---- | :--------- |
| Anna | geography | 3     | 1          |
| Anna | history   | 3     | 2          |
| Anna | math      | 4     | 3          |
| Anna | physics   | 5     | 4          |
| Bob  | geography | 4     | 1          |
| Bob  | history   | 4     | 2          |
| Bob  | physics   | 5     | 3          |

## Running total

A [↑ **running total**](https://learnsql.com/blog/what-is-a-running-total-and-how-to-compute-it-in-sql/) is the cumulative sum of the previous numbers in a column.

Look at the example below, which presents the daily registration of users for an online shop:

| registration_date | registered_users | total_users |
| ----------------- | ---------------- | ----------- |
| 2020-03-05        | 32               | 32          |
| 2020-03-06        | 15               | 47          |
| 2020-03-07        | 6                | 53          |

The first column shows the date. The second column shows the number of users who registered on that date. The third column, `total_users`, sums the total number of registered users on that day.

DDL:

```sql
CREATE TABLE user_registrations
(
    country           VARCHAR(50),
    registration_date DATE,
    registered_users  INTEGER
);
```

DML:

```sql
INSERT INTO user_registrations (country, registration_date, registered_users)
VALUES
    ('England', '2020-03-05', 25),
    ('England', '2020-03-06', 12),
    ('England', '2020-03-07', 10),
    ('Poland',  '2020-03-05', 32),
    ('Poland',  '2020-03-06', 15),
    ('Poland',  '2020-03-07', 6);
```

```sql
SELECT *
FROM user_registrations;
```

| country | registration_date | registered_users |
| :------ | :---------------- | :--------------- |
| England | 2020-03-05        | 25               |
| England | 2020-03-06        | 12               |
| England | 2020-03-07        | 10               |
| Poland  | 2020-03-05        | 32               |
| Poland  | 2020-03-06        | 15               |
| Poland  | 2020-03-07        | 6                |

Calculating the running total of registered users:

```sql
SELECT *,
       SUM(registered_users) OVER (PARTITION BY country ORDER BY registration_date)
FROM user_registrations;
```

| country | registration_date | registered_users | sum |
| :------ | :---------------- | :--------------- | :-- |
| England | 2020-03-05        | 25               | 25  |
| England | 2020-03-06        | 12               | 37  |
| England | 2020-03-07        | 10               | 47  |
| Poland  | 2020-03-05        | 32               | 32  |
| Poland  | 2020-03-06        | 15               | 47  |
| Poland  | 2020-03-07        | 6                | 53  |

Without `ORDER BY` similar query results in a regular sum (no running total calculated):

```sql
SELECT *,
       SUM(registered_users) OVER (PARTITION BY country)
FROM user_registrations;
```

| country | registration_date | registered_users | sum |
| :------ | :---------------- | :--------------- | :-- |
| England | 2020-03-05        | 25               | 47  |
| England | 2020-03-06        | 12               | 47  |
| England | 2020-03-07        | 10               | 47  |
| Poland  | 2020-03-05        | 32               | 53  |
| Poland  | 2020-03-06        | 15               | 53  |
| Poland  | 2020-03-07        | 6                | 53  |

## Moving average

A [↑ **moving average**](https://learnsql.com/blog/moving-average-in-sql/) is a technique used to smooth out short-term fluctuations in data to see long-term trends.

Instead of calculating the average of your entire dataset, you calculate the average for a specific "window" of rows that shifts as you move through the results.

It's most commonly used in time-series analysis, like tracking stock prices, daily website traffic, or temperature changes.

To calculate this in SQL, you use window functions. The query tells the database: "For the current row, look back at the last $X$ rows and calculate their average."

Below is the table named `stock_price`:

| date       | price |
| ---------- | ----- |
| 2020-06-20 | 132   |
| 2020-06-21 | 130   |
| 2020-06-23 | 130   |
| 2020-06-23 | 130   |
| 2020-06-24 | 108   |
| 2020-06-25 | 109   |
| 2020-06-26 | 106   |
| 2020-06-27 | 110   |
| 2020-06-28 | 117   |
| 2020-06-29 | 126   |
| 2020-06-30 | 120   |

Here is how a three-day moving average is calculated for January 9, 2020:

For January 9, 2020, the three-day moving average is calculated as the mean of prices from that day (130) and the two previous days: January 8 (130) and January 7 (132). So, the moving average for January 9, 2020 is the average of these three values (130 + 130 + 132) / 3 = 130.666.

The moving average is calculated in the same way for each of the remaining dates, totaling the three stock prices from the date in question and the two previous days then dividing that total by 3.
