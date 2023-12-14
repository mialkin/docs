# Query optimization and execution plans

SQL is a *declarative language*. That means that when we write a SQL query, we describe the result we want to get, but we do not specify *how* that result should be obtained. By contrast, in an *imperative language*, we specify *what* to do to obtain a desired result — that is, the sequence of steps that should be executed.

## Table of contents

- [Query optimization and execution plans](#query-optimization-and-execution-plans)
  - [Table of contents](#table-of-contents)
  - [Database optimizer](#database-optimizer)
  - [Execution plan](#execution-plan)
  - [`EXPLAIN` command](#explain-command)
  - [Short query](#short-query)
  - [Long query](#long-query)
  - [Optimization](#optimization)
  - [Postgres Air database](#postgres-air-database)
    - [Restore database](#restore-database)
    - [Set initial indexes](#set-initial-indexes)
  - [PostgreSQL specifics](#postgresql-specifics)
  - [Query processing overview](#query-processing-overview)
    - [Compilation](#compilation)
    - [Optimization and execution](#optimization-and-execution)
  - [Links](#links)

## Database optimizer

A **database optimizer** is a component of a database management system that chooses the best sequence of steps that should be executed in order to get result requested by a SQL query. What is best is determined by many different factors, such as storage structures, indexes, and data statistics.

The job of the optimizer is to build the best possible physical plan that implements a given logical plan. This is a complex process: sometimes, a complex logical operation is replaced with multiple physical operations, or several logical operations are merged into a single physical operation.

## Execution plan

An **execution plan** is the output of a database optimizer.

While a SQL query defines *what* needs to be done, an execution plan defines *how* to execute SQL operations.

A **plan space** is a set of possible execution plans for a query. Heuristics are used to reduce the number of plans evaluated by the optimizer.

## `EXPLAIN` command

In Postgres to obtain the execution plan for a query, the `EXPLAIN` command is run. This command takes any grammatically correct SQL statement as a parameter and returns its execution plan.

## Short query

A **short query** is a query for which the number of rows needed to compute its output is small, no matter how large the involved tables are.

Short queries may read every row from small tables but read only a small percentage of rows from large tables. How small is a "small percentage"?  Most of the time, however, it means less than 10%.

## Long query

A **long query** is a query selectivity of which is high for at least one of the large tables; that is, almost all rows contribute to the output, even when the output size is small.

In the majority of cases, in OLTP systems we are optimizing short queries and in
OLAP systems both short and long queries.

## Optimization

In the context of [↑ "PostgreSQL Query Optimization"](https://www.amazon.com/PostgreSQL-Query-Optimization-Ultimate-Efficient/dp/1484268849) book, optimization means any transformation that improves system performance. This definition is purposely very generic, since we want to emphasize that optimization is not a separate development phase. Quite often, database developers try to "just make it work" first and optimize later. We do not think that this approach is productive. Writing a query without having any idea of how long it will take to run creates a problem that could have been avoided altogether by writing it the right way from the start.

The most important thing is to understand how a database engine processes a query and how a query planner decides what execution path to choose. When we teach optimization in a classroom setting, we often say, "Think like a database!" Look at your query from the point of view of a database engine, and imagine what it has to do to execute that query; imagine that you have to do it yourself instead of the database engine doing it for you.

If you practice "thinking like a database" long enough, it will become a natural way of thinking, and you will be able to write queries correctly right away, often without the need for future optimization.

Writing a database query is different from writing application code using imperative languages. SQL is a declarative language, which means that we specify the desired outcome, but do not specify an execution path. Since two queries yielding the same result may be executed differently, utilizing different resources and taking a different amount of time, optimization and "thinking like a database" are core parts of SQL development.

## Postgres Air database

Throughout [↑ "PostgreSQL Query Optimization"](https://www.amazon.com/PostgreSQL-Query-Optimization-Ultimate-Efficient/dp/1484268849) book, examples are built on one of the databases of a virtual airline company called Postgres Air.

This company connects over 600 virtual destinations worldwide, offers about 32,000 direct virtual flights weekly, and has over 100,000 virtual members in its frequent flyer program and many more passengers every week. The company fleet consists of virtual aircraft.

### Restore database

Google Drive [↑ link](https://drive.google.com/drive/folders/13F7M80Kf_somnjb-mTYAnh1hW1Y_g4kJ) for downloading database backup.

```bash
docker exec -i postgres pg_restore \
-U postgres \
-v \
-d postgres_air /Users/john.doe/Downloads/postgres_air_2023.backup

docker exec postgres sh -c "pg_restore -C -d postgres_air /Users/john.doe/Downloads/postgres_air_2023.backup"

docker exec -i postgres pg_restore -U postgres -v -d postgres_air < /Users/john.doe/Downloads/postgres_air_2023.backup
```

### Set initial indexes

```sql
set search_path to postgres_air;

CREATE INDEX flight_departure_airport ON flight (departure_airport);
CREATE INDEX flight_scheduled_departure ON flight (scheduled_departure);
CREATE INDEX flight_update_ts ON flight (update_ts);
CREATE INDEX booking_leg_booking_id ON booking_leg (booking_id);
CREATE INDEX booking_leg_update_ts ON booking_leg (update_ts);
CREATE INDEX account_last_name ON account (last_name);
```

## PostgreSQL specifics

Perhaps the most important feature you should be aware of is that PostgreSQL does not have optimizer hints. If you previously worked with a database like Oracle, which does have the option of "hinting" to the optimizer, you might feel helpless when you
are presented with the challenge of optimizing a PostgreSQL query. However, here is some good news: PostgreSQL does not have hints by design. The PostgreSQL core team believes in investing in developing a query planner which is capable of choosing the best execution path without hints. As a result, the PostgreSQL optimization engine is one of the best among both commercial and open source systems. Many strong database internal developers have been drawn to Postgres because of the optimizer. In addition, Postgres has been chosen as the founding source code base for several commercial databases partly because of the optimizer. With PostgreSQL, it is even more important to write your SQL statements declaratively, allowing the optimizer to do its job.

Another PostgreSQL feature you should be aware of is the difference between the execution of parameterized queries and dynamic SQL.

With PostgreSQL, it is especially important to be aware of new features and capabilities added with each release. In recent years, Postgres has had over 180 of them each year. Many of these features are around optimization. PostgreSQL has an incredibly rich set of types and indexes, and it is always worth consulting recent documentation to check whether a feature you wanted might have been implemented.

## Query processing overview

In order to produce query results, PostgreSQL performs the following steps:

- Compile and transform a SQL statement into an expression consisting of high-level logical operations, known as a **logical plan**
- Optimize the logical plan and convert it into an execution plan
- Execute (interpret) the plan and return results

### Compilation

Compiling a SQL query is similar to compiling code written in an imperative language. The source code is parsed, and an internal representation is generated. However, the compilation of SQL statements has two essential differences:

- In an imperative language, the definitions of identifiers are usually included in the source code, while definitions of objects referenced in SQL queries are mostly stored in the database. Consequently, the meaning of a query depends on the database structure: different database servers can interpret the same query differently.
- The output of an imperative language compiler is usually (almost) executable code, such as byte code for a Java virtual machine. In contrast, the output of a query compiler is an expression consisting of high-level operations that remain declarative—they do not give any instruction on how to obtain the required output. A possible order of operations is specified at this point, but not the manner of executing those operations.

### Optimization and execution

An optimizer performs two kinds of transformations: it replaces logical operations with their execution algorithms and possibly changes the logical expression structure by changing the order in which logical operations will be executed.

Neither of these transformations is straightforward; a logical operation can be computed using different algorithms, and the optimizer tries to choose the best one. The same query may be represented with several equivalent expressions producing the same result but requiring a significantly different amount of computational resources for execution. The optimizer tries to find a logical plan and physical operations that minimize required resources, including execution time. This search requires sophisticated algorithms that are out of scope for this conversation.

The output of the optimizer is an expression containing physical operations. This expression is called a (**physical**) **execution plan**. For that reason, the PostgreSQL optimizer is called the **query planner**.

Finally, the query execution plan is interpreted by the **query execution engine**, frequently referred to as the **executor** in the PostgreSQL community, and output is returned to the client application.

## Links

[↑ Execution Plan Basics](https://www.red-gate.com/simple-talk/databases/sql-server/performance-sql-server/execution-plan-basics/).

[↑ Exploring Query Plans in SQL](https://www.red-gate.com/simple-talk/databases/sql-server/database-administration-sql-server/exploring-query-plans-in-sql/).
