# Window function

A **window function** is a function that performs a calculation across a set of table rows that are somehow related to the current row.

The term *window* describes the set of rows on which the function operates. A window function uses values from the rows in a window to calculate the returned values.

## Table of contents

- [Window function](#window-function)
  - [Table of contents](#table-of-contents)
  - [Syntax overview](#syntax-overview)
  - [Initialization](#initialization)
  - [Average salary in department](#average-salary-in-department)
  - [Window functions vs `GROUP BY`](#window-functions-vs-group-by)

## Syntax overview

```text
function (expression) over 
(
    [ partition by expression_list ]
    [ order by order_list ]
    [ rows frame_clause ]
)
```

Window functions are initiated with the `OVER` clause, and are configured using three concepts:

- Window partition (`PARTITION BY`) — groups rows into partitions
- Window ordering (`ORDER BY`) — defines the order or sequence of rows within each window
- Window frame (`ROWS`) — defines the window by use of an offset from the specified row

## Initialization

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

## Average salary in department

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

## Window functions vs `GROUP BY`

- Window functions don't reduce the number of rows in the output.
- Window functions can retrieve values from other rows, whereas `GROUP BY` functions cannot.
- Window functions can calculate running totals and moving averages, whereas `GROUP BY` functions cannot.
