# N + 1 query problem. Natural, surrogate key

## Table of contents

- [N + 1 query problem. Natural, surrogate key](#n--1-query-problem-natural-surrogate-key)
  - [Table of contents](#table-of-contents)
  - [N + 1 query problem](#n--1-query-problem)
  - [Natural, surrogate key](#natural-surrogate-key)

## N + 1 query problem

The **N + 1 query problem** is a performance issue that occurs in database-driven applications, particularly when using ORM frameworks.

It happens when the application needs to load a collection of related data, and instead of fetching all the necessary data in a single query, it executes one query to retrieve a list of items (N queries) and then executes additional queries to fetch related data for each item in the list (1 query per item). This results in a total of N + 1 queries.

Let's say we have a collection of `Car` objects (database rows), and each `Car` has a collection of `Wheel` objects (also rows). In other words, `Car` → `Wheel` is a 1-to-many relationship.

Now, let's say we need to iterate through all the cars, and for each one, print out a list of the wheels. The naive object–relational mapping implementation would do the following:

```sql
select * from Cars;
```

And then *for each* `Car`:

```sql
select * from Wheels where CarId = ?
```

In other words, we have one select for the cars, and then `N` additional selects, where `N` is the total number of cars.

Alternatively, we could get all the wheels and perform the lookups in memory:

```sql
select * from Wheels;
```

This reduces the number of round-trips to the database from `N + 1` to `2`.

## Natural, surrogate key

A **natural key** is a type of unique key in a database formed of attributes that exist and are used in the external world outside the database, for example in the business domain.

Usage of natural keys as unique identifiers in a table has one main disadvantage which is the change of business rules or the change of rules of the attribute in the real world. For example if there is a table storing the information about US citizens, the SSN would act as the natural key. If the US government changes the structure of the SSN and increases the number of digits in the SSN due to some reason.

A **surrogate key** in a database is a unique identifier for either an entity in the modeled world or an object in the database. The surrogate key is not derived from application data, unlike a natural key.

In a current database, the surrogate key can be the primary key, generated by the database management system and not derived from any application data in the database. The only significance of the surrogate key is to act as the primary key.