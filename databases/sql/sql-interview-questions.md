# SQL interview questions

## `PARTITION BY`

You have an `employees` table with fields: `name`, `department`, and `salary`. Write a query to find the top-N highest-paid employees in each department.

How would you handle cases where multiple employees in the same department have the exact same top salary?

```sql
CREATE TABLE IF NOT EXISTS employees
(
    name       text    NOT NULL PRIMARY KEY,
    department text    NOT NULL,
    salary     decimal NOT NULL
);

INSERT INTO employees
VALUES ('Anna', 'Development', 300),
       ('Victoria', 'Sales', 200),
       ('Boris', 'Development', 300),
       ('Vicky', 'Development', 250),
       ('Pyotr', 'Sales', 150),
       ('Vladimir', 'Development', 250),
       ('Aleksei', 'Sales', 150),
       ('Evgenia', 'Development', 200),
       ('Elena', 'Logistics', 200),
       ('Rita', 'Sales', 125),
       ('Irina', 'Sales', 115),
       ('Igor', 'Development', 100);
```

The "trap" in this interview question is that simply selecting `MAX(salary) GROUP BY department` gives you the numbers, but it _cannot_ give you the _names_ associated with those numbers:

```sql
SELECT department, MAX(salary)
FROM employees
GROUP BY department
ORDER BY MAX(salary) DESC;
```

