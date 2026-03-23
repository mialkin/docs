# CTE, `WITH`. `HAVING`, `FULL JOIN`

## Table of contents

- [CTE, `WITH`. `HAVING`, `FULL JOIN`](#cte-with-having-full-join)
  - [Table of contents](#table-of-contents)
  - [CTE, `WITH`](#cte-with)
    - [Basic CTE](#basic-cte)
  - [Temporary table](#temporary-table)
  - [`HAVING`](#having)
  - [Join types](#join-types)
    - [`FULL JOIN`](#full-join)
    - [`CROSS JOIN`](#cross-join)

## CTE, `WITH`

A **common table expression** or **CTE** is a temporary result set defined with `WITH`. Think of it as readable, named bucket for your data that exists only during the execution of that single query.

CTEs are much cleaner than nested subqueries because they follow a logical "top-to-bottom" flow. Instead of burying a subquery inside your `FROM` clause, you define the logic at the top using the `WITH` keyword.

### Basic CTE

**Scenario**: You want to find all employees who earn more than the average salary.

```sql
WITH AverageSalaryCTE AS (SELECT AVG(Salary) AS AvgSal
                          FROM Employees)
SELECT Name, Salary
FROM Employees,
     AverageSalaryCTE
WHERE Salary > AvgSal;
```

**Why use it?** It separates the "calculation" step from the "display" step, making the code much easier for a teammate (or future you) to read. Compare it with:

```sql
SELECT Name, Salary
FROM Employees
WHERE Salary > (SELECT AVG(Salary) AS AvgSal
                FROM Employees);
```

Each auxiliary statement in a `with` clause can be a `select`, `insert`, `update`, or `delete`. The `with` clause itself is attached to a primary statement that can also be a `select`, `insert`, `update`, or `delete`.

Question:

```text
When there are several CTEs inside of a single query do they execute sequentially?
```

The short answer is: logically, yes; physically, not necessarily. In SQL, there is a big difference between how you _write_ the code (the logical flow) and how the database _runs_ the code (the execution plan).

Pro-tip: If you have a CTE that performs a very heavy calculation and you use it three different times in your main query, some engines might accidentally run that heavy calculation three times. In those cases, using a [temporary table](#temporary-table) instead of a CTE can be faster because it forces the data to be saved once.

[↑ PostgreSQL `WITH` queries](https://www.postgresql.org/docs/16/queries-with.html).

[↑ T-SQL `WITH common_table_expression`](https://learn.microsoft.com/en-us/sql/t-sql/queries/with-common-table-expression-transact-sql).

## Temporary table

?

## `HAVING`

The **`HAVING`** clause filters rows after the aggregation of the `GROUP BY` clause has been applied.

A `HAVING` clause must come after any `GROUP BY` clause and before any `ORDER BY` clause.

A `HAVING` is like `WHERE` clause, but applies to groups. Results from a `HAVING` clause represent groupings or aggregations of original rows, whereas results from a `WHERE` clause are individual original rows.

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

## Join types

Initialization script:

```sql
BEGIN TRANSACTION;

CREATE TABLE t1
(
    num  integer NOT NULL,
    name varchar NOT NULL
);


CREATE TABLE t2
(
    num   integer NOT NULL,
    value varchar NOT NULL
);

INSERT INTO t1 (num, name)
VALUES (1, 'a'),
       (2, 'b'),
       (3, 'c');


INSERT INTO t2 (num, value)
VALUES (1, 'xxx'),
       (3, 'yyy'),
       (5, 'zzz');

COMMIT;
```

`t1` table:

| num | name |
| :-- | :--- |
| 1   | a    |
| 2   | b    |
| 3   | c    |

`t2` table:

| num | value |
| :-- | :---- |
| 1   | xxx   |
| 3   | yyy   |
| 5   | zzz   |

### `FULL JOIN`

First, an inner join is performed. Then, for each row in `t1` that does not satisfy the join condition with any row in `t2`, a joined row is added with null values in columns of `t2`. Also, for each row of `t2` that does not satisfy the join condition with any row in `t1`, a joined row with null values in the columns of `t1` is added.

```sql
SELECT *
FROM t1
         FULL JOIN t2 ON t1.num = t2.num;
```

| num | name | num | value |
| :-- | :--- | :-- | :---- |
| 1   | a    | 1   | xxx   |
| 2   | b    |     |       |
| 3   | c    | 3   | yyy   |
|     |      | 5   | zzz   |

[↑ Queries join](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-JOIN).

### `CROSS JOIN`

For each combination of rows from `t1` and `t2`, the derived table will contain a row consisting of all columns in `t1` followed by all columns in `t2`. If the tables have `N` and `M` rows respectively, the joined table will have `N` $\times$ `M` rows.

```sql
SELECT *
FROM t1
         CROSS JOIN t2;
```

| num | name | num | value |
| :-- | :--- | :-- | :---- |
| 1   | a    | 1   | xxx   |
| 1   | a    | 3   | yyy   |
| 1   | a    | 5   | zzz   |
| 2   | b    | 1   | xxx   |
| 2   | b    | 3   | yyy   |
| 2   | b    | 5   | zzz   |
| 3   | c    | 1   | xxx   |
| 3   | c    | 3   | yyy   |
| 3   | c    | 5   | zzz   |
