# N + 1 Query Problem

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
