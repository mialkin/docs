# SQL interview questions

## Find the top-N highest-paid employees in each department

You have an `employees` table with fields: `name`, `department`, and `salary`. Write a query to find the top-N highest-paid employees in each department.

DDL:

```sql
CREATE TABLE IF NOT EXISTS employees
(
    name       text    NOT NULL PRIMARY KEY,
    department text    NOT NULL,
    salary     decimal NOT NULL
);
```

DML:

```sql
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

First, let's get all the employees from each department sorted by their salary:

```sql
SELECT *,
       ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rank
FROM employees
```

| name     | department  | salary | rank |
| :------- | :---------- | :----- | :--- |
| Anna     | Development | 300    | 1    |
| Boris    | Development | 300    | 2    |
| Vladimir | Development | 250    | 3    |
| Vicky    | Development | 250    | 4    |
| Evgenia  | Development | 200    | 5    |
| Igor     | Development | 100    | 6    |
| Elena    | Logistics   | 200    | 1    |
| Victoria | Sales       | 200    | 1    |
| Aleksei  | Sales       | 150    | 2    |
| Pyotr    | Sales       | 150    | 3    |
| Rita     | Sales       | 125    | 4    |
| Irina    | Sales       | 115    | 5    |
