# Common table expression

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
