# Window functions, `OVER`, `PARTITION BY`

A **window function** in SQL is a function that operates on a selected set of rows (a _window_, _partition_) and performs a calculation for that set of rows in a separate column.

```sql
CREATE TABLE student_grades
(
    name    varchar,
    subject varchar,
    grade   int
);
```

```sql
INSERT INTO student_grades (VALUES ('Петя', 'русский', 4),
                                   ('Петя', 'физика', 5),
                                   ('Петя', 'история', 4),
                                   ('Маша', 'математика', 4),
                                   ('Маша', 'русский', 3),
                                   ('Маша', 'физика', 5),
                                   ('Маша', 'история', 3));
```
