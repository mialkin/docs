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

## Optimization

In the majority of cases, in OLTP systems we are optimizing short queries and in
OLAP systems both short and long queries.

## Postgres Air database

Throughout [↑ "PostgreSQL Query Optimization"](https://www.amazon.com/PostgreSQL-Query-Optimization-Ultimate-Efficient/dp/1484268849) book, examples are built on one of the databases of a virtual airline company called Postgres Air.

This company connects over 600 virtual destinations worldwide, offers about 32,000 direct virtual flights weekly, and has over 100,000 virtual members in its frequent flyer program and many more passengers every week. The company fleet consists of virtual aircraft.

Google Drive [↑ link](https://drive.google.com/drive/folders/13F7M80Kf_somnjb-mTYAnh1hW1Y_g4kJ) for downloading database backup.

```bash
docker exec -i postgres pg_restore \
-U postgres \
-v \
-d postgres_air /Users/john.doe/Downloads/postgres_air_2023.backup


docker exec postgres sh -c "pg_restore -C -d postgres_air /Users/john.doe/Downloads/postgres_air_2023.backup"

docker exec -i postgres pg_restore -U postgres -v -d postgres_air < /Users/john.doe/Downloads/postgres_air_2023.backup
```
