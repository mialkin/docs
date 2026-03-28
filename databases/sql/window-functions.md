# Window functions, `OVER`, `PARTITION BY`, running total, moving average

A **window function** in SQL is a function that operates on a selected set of rows (a _window_, _partition_) and
performs a calculation for that set of rows in a separate column.

For a function (like `SUM()`, `RANK()`, etc) to actually behave as a "window function," it must be followed by the
`OVER` clause.

A window function performs a calculation across a set of table rows that are somehow related to the current row. This is
comparable to the type of calculation that can be done with an aggregate function. However, window functions do not
cause rows to become grouped into a single output row like non-window aggregate calls would. Instead, the rows retain
their separate identities. Behind the scenes, the window function is able to access more than just the current row of
the query result.

## Table of contents

- [Window functions, `OVER`, `PARTITION BY`, running total, moving average](#window-functions-over-partition-by-running-total-moving-average)
  - [Table of contents](#table-of-contents)
  - [DDL \& DML](#ddl--dml)
  - [3 classes of window functions](#3-classes-of-window-functions)
    - [Aggregating](#aggregating)
    - [Ranking](#ranking)
    - [Value](#value)
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

`ROW_NUMBER()` function returns the sequential number of a row within a partition of a result set, starting at 1 for the
first row in each partition.

`NTILE(N)` function receives an integer parameter (`N`) and divides the complete set of rows into `N` subsets. Each
subset has approximately the same number of rows and is identified by a number between 1 and `N`. This ID number is what
`NTILE()` returns.

`RANK()` function gives you the ranking within your ordered partition. Ties are assigned the same rank, with the next
ranking(s) skipped. So, if you have 3 items at rank 2, the next rank listed would be ranked 5.

`DENSE_RANK()` function gives you the ranking within your ordered partition, but the ranks are consecutive. No ranks are
skipped if there are ranks with multiple items. So, if you have 3 items at rank 2, the next rank listed would be ranked

`CUME_DIST()` function calculates the cumulative distribution of a value within a group of values.

Expert Tip: While `CUME_DIST()` is listed under Ranking, some documentation might group it under "Distribution
Functions" along with `PERCENT_RANK()`. However, placing it under Ranking is still contextually correct for most SQL
learners.

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

### Value

DDL:

```sql
CREATE TABLE student_quarter
(
    name    varchar,
    quarter varchar,
    subject varchar,
    grade   int
);
```

DML:

```sql
INSERT INTO student_quarter (VALUES ('Anna', '1 quarter', 'physics', 4),
                                    ('Anna', '2 quarter', 'physics', 3),
                                    ('Anna', '3 quarter', 'physics', 4),
                                    ('Anna', '4 quarter', 'physics', 5));
```

```sql
SELECT *
FROM student_quarter;
```

| name | quarter   | subject | grade |
| :--- | :-------- | :------ | :---- |
| Anna | 1 quarter | physics | 4     |
| Anna | 2 quarter | physics | 3     |
| Anna | 3 quarter | physics | 4     |
| Anna | 4 quarter | physics | 5     |

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

A [↑ **running total**](https://learnsql.com/blog/what-is-a-running-total-and-how-to-compute-it-in-sql/) is the
cumulative sum of the previous numbers in a column.

Look at the example below, which presents the daily registration of users for an online shop:

| registration_date | registered_users | total_users |
| ----------------- | ---------------- | ----------- |
| 2020-03-05        | 32               | 32          |
| 2020-03-06        | 15               | 47          |
| 2020-03-07        | 6                | 53          |

The first column shows the date. The second column shows the number of users who registered on that date. The third
column, `total_users`, sums the total number of registered users on that day.

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
VALUES ('England', '2020-03-05', 25),
       ('England', '2020-03-06', 12),
       ('England', '2020-03-07', 10),
       ('Poland', '2020-03-05', 32),
       ('Poland', '2020-03-06', 15),
       ('Poland', '2020-03-07', 6);
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

A [↑ **moving average**](https://learnsql.com/blog/moving-average-in-sql/) is a technique used to smooth out short-term
fluctuations in data to see long-term trends.

Instead of calculating the average of your entire dataset, you calculate the average for a specific "window" of rows
that shifts as you move through the results.

It's most commonly used in time-series analysis, like tracking stock prices, daily website traffic, or temperature
changes.

To calculate this in SQL, you use window functions. The query tells the database: "For the current row, look back at the
last $X$ rows and calculate their average."

Below is the table named `price_history`:

```sql
SELECT *
FROM price_history;
```

| id  | price_date | price  |
| :-- | :--------- | :----- |
| 1   | 2020-06-20 | 132.00 |
| 2   | 2020-06-21 | 130.00 |
| 3   | 2020-06-23 | 110.00 |
| 4   | 2020-06-23 | 120.00 |
| 5   | 2020-06-24 | 108.00 |
| 6   | 2020-06-25 | 109.00 |
| 7   | 2020-06-26 | 106.00 |
| 8   | 2020-06-27 | 110.00 |
| 9   | 2020-06-28 | 117.00 |
| 10  | 2020-06-29 | 126.00 |
| 11  | 2020-06-30 | 120.00 |

DDL:

```sql
CREATE TABLE price_history
(
  id         INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  price_date DATE           NOT NULL,
  price      DECIMAL(12, 2) NOT NULL
);
```

DML:

```sql
INSERT INTO price_history (price_date, price)
VALUES ('2020-06-20', 132),
       ('2020-06-21', 130),
       ('2020-06-23', 110),
       ('2020-06-23', 120),
       ('2020-06-24', 108),
       ('2020-06-25', 109),
       ('2020-06-26', 106),
       ('2020-06-27', 110),
       ('2020-06-28', 117),
       ('2020-06-29', 126),
       ('2020-06-30', 120);
```

Here is a moving average `SELECT`:

```sql
SELECT *,
       AVG(price) OVER (ORDER BY price_date)
FROM price_history;
```

| id  | price_date | price  | avg                  |
| :-- | :--------- | :----- | :------------------- |
| 1   | 2020-06-20 | 132.00 | 132                  |
| 2   | 2020-06-21 | 130.00 | 131                  |
| 3   | 2020-06-23 | 110.00 | 123                  |
| 4   | 2020-06-23 | 120.00 | 123                  |
| 5   | 2020-06-24 | 108.00 | 120                  |
| 6   | 2020-06-25 | 109.00 | 118.1666666666666667 |
| 7   | 2020-06-26 | 106.00 | 116.4285714285714286 |
| 8   | 2020-06-27 | 110.00 | 115.625              |
| 9   | 2020-06-28 | 117.00 | 115.7777777777777778 |
| 10  | 2020-06-29 | 126.00 | 116.8                |
| 11  | 2020-06-30 | 120.00 | 117.0909090909090909 |

For the row with ID = 2 moving average is calculated as (132+130)/2 = 131.

For the row with ID = 6 moving average is calculated as (132+130+110+120+108+109)/6 = 118.

Here is an example that uses `ROWS BETWEEN N PRECEDING AND CURRENT ROW`:

```sql
SELECT *,
       AVG(price) OVER (ORDER BY price_date ROWS BETWEEN 3 PRECEDING AND CURRENT ROW) AS three_day_moving_average,
       AVG(price) OVER (ORDER BY price_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS one_week_moving_average
FROM price_history;
```

| id  | price_date | price  | three_day_moving_average | one_week_moving_average |
| :-- | :--------- | :----- | :----------------------- | :---------------------- |
| 1   | 2020-06-20 | 132.00 | 132                      | 132                     |
| 2   | 2020-06-21 | 130.00 | 131                      | 131                     |
| 3   | 2020-06-23 | 110.00 | 124                      | 124                     |
| 4   | 2020-06-23 | 120.00 | 123                      | 123                     |
| 5   | 2020-06-24 | 108.00 | 117                      | 120                     |
| 6   | 2020-06-25 | 109.00 | 111.75                   | 118.1666666666666667    |
| 7   | 2020-06-26 | 106.00 | 110.75                   | 116.4285714285714286    |
| 8   | 2020-06-27 | 110.00 | 108.25                   | 113.2857142857142857    |
| 9   | 2020-06-28 | 117.00 | 110.5                    | 111.4285714285714286    |
| 10  | 2020-06-29 | 126.00 | 114.75                   | 113.7142857142857143    |
| 11  | 2020-06-30 | 120.00 | 118.25                   | 113.7142857142857143    |

For the row with ID = 2 `three_day_moving_average` and `one_week_moving_average` is calculated as (132+130)/2 = 131.

For the row with ID = 7 `three_day_moving_average` is calculated as (120+108+109+106)/4 = 110.75 and
`one_week_moving_average` is calculated as (132+130+110+120+108+109+106)/7 = 116.42.
