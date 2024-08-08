# CTE. Window function. `HAVING`

## Table of contents

- [CTE. Window function. `HAVING`](#cte-window-function-having)
  - [Table of contents](#table-of-contents)
  - [CTE](#cte)
  - [Window function](#window-function)
    - [Window functions vs `GROUP BY`](#window-functions-vs-group-by)
  - [`HAVING`](#having)

## CTE

A **common table expressions** or **CTE** is an auxiliary statement which is used in a larger query.

CTE, can be thought of as defining temporary table that exists just for one query.

Each auxiliary statement in a `with` clause can be a `select`, `insert`, `update`, or `delete`. The `with` clause itself is attached to a primary statement that can also be a `select`, `insert`, `update`, or `delete`.

Example of a CTE:

```sql
with whatever as (
    select *
    from employee_salary
),
somethingelse as
    (
        select id, department
        from employee_salary
    )
select whatever.id, whatever.salary, somethingelse.department
from whatever
join somethingelse on somethingelse.id = whatever.id;
```

[↑ PostgreSQL WITH Queries](https://www.postgresql.org/docs/16/queries-with.html).

[↑ T-SQL WITH common_table_expression](https://learn.microsoft.com/en-us/sql/t-sql/queries/with-common-table-expression-transact-sql).

## Window function

A [↑ **window function**](<https://en.wikipedia.org/wiki/Window_function_(SQL)>) is a function that performs a calculation across a set of table rows that are somehow related to the current row.

The term _window_ describes the set of rows on which the function operates. A window function uses values from the rows in a window to calculate the returned values.

Syntax overview:

```sql
function (expression) OVER
(
    [ PARTITION BY expression_list ]
    [ ORDER BY order_list ]
    [ ROWS frame_clause ]
)
```

Window functions are initiated with the `OVER` clause, and are configured using three concepts:

- Window partition (`PARTITION BY`) — groups rows into partitions
- Window ordering (`ORDER BY`) — defines the order or sequence of rows within each window
- Window frame (`ROWS`) — defines the window by use of an offset from the specified row

Initialization script for PostgreSQL:

```sql
CREATE TABLE employee_salary
(
    "id"              bigserial PRIMARY KEY,
    "department"      varchar NOT NULL,
    "employee_number" bigint  NOT NULL,
    "salary"          bigint  NOT NULL
);

INSERT INTO employee_salary (id, department, employee_number, salary)
VALUES (1, 'development', 11, 5200),
       (2, 'development', 7, 4200),
       (3, 'development', 9, 4500),
       (4, 'development', 8, 6000),
       (5, 'development', 10, 5200),
       (6, 'personnel', 5, 3500),
       (7, 'personnel', 2, 3900),
       (8, 'sales', 3, 4800),
       (9, 'sales', 1, 5000),
       (10, 'sales', 4, 4800);
```

Resulting table:

```sql
SELECT *
FROM employee_salary;
```

| id  | department  | employee_number | salary |
| --- | ----------- | --------------- | ------ |
| 1   | development | 11              | 5200   |
| 2   | development | 7               | 4200   |
| 3   | development | 9               | 4500   |
| 4   | development | 8               | 6000   |
| 5   | development | 10              | 5200   |
| 6   | personnel   | 5               | 3500   |
| 7   | personnel   | 2               | 3900   |
| 8   | sales       | 3               | 4800   |
| 9   | sales       | 1               | 5000   |
| 10  | sales       | 4               | 4800   |

Average salary in department:

```sql
SELECT department, employee_number, salary, AVG(salary) OVER (PARTITION BY department)
FROM employee_salary;
```

| id  | department  | employee_number | salary | avg                   |
| --- | ----------- | --------------- | ------ | --------------------- |
| 1   | development | 11              | 5200   | 5020                  |
| 2   | development | 7               | 4200   | 5020                  |
| 3   | development | 9               | 4500   | 5020                  |
| 4   | development | 8               | 6000   | 5020                  |
| 5   | development | 10              | 5200   | 5020                  |
| 6   | personnel   | 5               | 3500   | 3700                  |
| 7   | personnel   | 2               | 3900   | 3700                  |
| 8   | sales       | 3               | 4800   | 4866.6666666666666667 |
| 9   | sales       | 1               | 5000   | 4866.6666666666666667 |
| 10  | sales       | 4               | 4800   | 4866.6666666666666667 |

### Window functions vs `GROUP BY`

- Window functions don't reduce the number of rows in the output.
- Window functions can retrieve values from other rows, whereas `GROUP BY` functions cannot.
- Window functions can calculate _running totals_ and _moving averages_, whereas `GROUP BY` functions cannot.

A [↑ **running total**](https://learnsql.com/blog/what-is-a-running-total-and-how-to-compute-it-in-sql/) is the cumulative sum of the previous numbers in a column.

Look at the example below, which presents the daily registration of users for an online shop:

| registration_date | registered_users | total_users |
| ----------------- | ---------------- | ----------- |
| 2020-03-05        | 32               | 32          |
| 2020-03-06        | 15               | 47          |
| 2020-03-07        | 6                | 53          |

The first column shows the date. The second column shows the number of users who registered on that date. The third column, total_users, sums the total number of registered users on that day.

A [↑ **moving average**](https://learnsql.com/blog/moving-average-in-sql/) is a time series technique for analyzing and determining trends in data.

Sometimes called rolling means, rolling averages, or running averages, they are calculated as the mean of the current and a specified number of immediately preceding values for each point in time.

Below is the table named `stock_price`:

| date       | price |
| ---------- | ----- |
| 2020-01-07 | 132   |
| 2020-01-08 | 130   |
| 2020-01-09 | 130   |
| 2020-01-10 | 130   |
| ...        | ...   |
| 2020-06-24 | 108   |
| 2020-06-25 | 109   |
| 2020-06-26 | 106   |
| 2020-06-27 | 106   |
| 2020-06-28 | 107   |
| 2020-06-29 | 106   |
| 2020-06-30 | 106   |

Here is how a three-day moving average is calculated for January 9, 2020:

<img src="moving-average.jpeg" width="600px" alt="Moving average" />

For January 9, 2020, the three-day moving average is calculated as the mean of prices from that day (1,300) and the two previous days: January 8 (1,300) and January 7 (1,320). So, the moving average for January 9, 2020 is the average of these three values, or 1,306.66 as shown in the image above.

The moving average is calculated in the same way for each of the remaining dates, totaling the three stock prices from the date in question and the two previous days then dividing that total by 3.

## `HAVING`

```sql
SELECT fare_conditions, COUNT(*) AS total
FROM seats
GROUP BY fare_conditions
```

| fare_conditions | total |
| :-------------- | :---- |
| Business        | 152   |
| Comfort         | 48    |
| Economy         | 1139  |

```sql
SELECT fare_conditions, COUNT(*) AS total
FROM seats
GROUP BY fare_conditions
HAVING fare_conditions != 'Comfort'
   AND COUNT(*) < 500;
```

| fare_conditions | total |
| :-------------- | :---- |
| Business        | 152   |

The [↑ `HAVING`](https://www.postgresql.org/docs/current/tutorial-agg.html) clause was added to SQL because the `WHERE` clause cannot be used with aggregate functions.

Aggregate functions are often used with `GROUP BY` clauses, and by adding `HAVING` we can write condition like we do with `WHERE` clauses.
