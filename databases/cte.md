# Common Table Expressions

The **common table expressions** or **CTE**s are the auxiliary statements which are used in a larger query. CTEs, can be thought of as defining temporary tables that exist just for one query.

Each auxiliary statement in a `with` clause can be a `select`, `insert`, `update`, or `delete`; and the `with` clause itself is attached to a primary statement that can also be a `select`, `insert`, `update`, or `delete`.

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

## Links

<https://www.postgresql.org/docs/9.1/queries-with.html>

<https://docs.microsoft.com/en-us/sql/t-sql/queries/with-common-table-expression-transact-sql>
