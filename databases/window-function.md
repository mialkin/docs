# Window function

A **window function** is a function that performs a calculation across a set of table rows that are somehow related to the current row.

The term **window** describes the set of rows on which the function operates. A window function uses values from the rows in a window to calculate the returned values.

## Syntax overiew

```text
function (expression) over 
(
    [ partition by expression_list ]
    [ order by order_list ]
    [ rows frame_clause ]
)
```

Window functions are initiated with the `over` clause, and are configured using three concepts:

- Window partition (`partition by`) — groups rows into partitions
- Window ordering (`order by`) — defines the order or sequence of rows within each window
- Window frame (`rows`) — defines the window by use of an offset from the specified row

## Initialization

Initialization script for PostgreSQL:

```sql
create table "employee_salary"
(
    "id"              bigserial primary key,
    "department"      varchar not null,
    "employee_number" bigint  not null,
    "salary"          bigint  not null
);

insert into employee_salary (id, department, employee_number, salary)
values (1, 'development', 11, 5200),
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
select * from employee_salary;
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

## Average salary in employee's department

```sql
select department, employee_number, salary, avg(salary) over (partition by department)
from employee_salary;
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

## Window functions vs `group by`

- Window functions don't reduce the number of rows in the output.
- Window functions can retrieve values from other rows, whereas `group by` functions cannot.
- Window functions can calculate running totals and moving averages, whereas `group by` functions cannot.

## Links

- [3.5. Window Functions](https://www.postgresql.org/docs/current/tutorial-window.html)
- [Оконные функции в SQL — что это и зачем они нужны](https://tproger.ru/translations/sql-window-functions/)
- [Convert CSV to Markdown](https://www.convertcsv.com/csv-to-markdown.htm)
- [Учимся применять оконные функции](http://thisisdata.ru/blog/uchimsya-primenyat-okonnyye-funktsii/)
- https://stackoverflow.com/questions/8477040/highest-salary-in-each-department/8477093
- https://stackoverflow.com/questions/4297160/sql-command-for-finding-the-second-highest-salary
