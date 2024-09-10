# Query execution

## Table of contents

- [Query execution](#query-execution)
  - [Table of contents](#table-of-contents)
  - [Stages of query processing](#stages-of-query-processing)
  - [JIT compilation](#jit-compilation)
  - [Prepared operators](#prepared-operators)
  - [Cursors](#cursors)
  - [More about planning](#more-about-planning)

## Stages of query processing

- Parsing
- Rewriting or transformation
- Planning or optimization
- Execution

Execution is similar to pipeline processing. Execution tree is walked from bottom nodes to top nodes. Some nodes can process data immediately (nested loop join), while others have to wait for all the data (sorting).

Joining happens in pairs. If three tables need to be joined, then the first two tables are joined and later the last one joined with the result.

```sql
EXPLAIN (ANALYZE, COSTS OFF, TIMING OFF )
SELECT *
FROM ticket_flights;
```

```console
Seq Scan on ticket_flights (actual rows=8391852 loops=1)
Planning Time: 0.094 ms
Execution Time: 458.981 ms
```

It's possible to retrieve more information about each stage in Postgres logs by turning on some settings:

```sql
ALTER SYSTEM SET LOG_PARSER_STATS = on;
ALTER SYSTEM SET LOG_PLANNER_STATS = on;
ALTER SYSTEM SET LOG_EXECUTOR_STATS = on;

SELECT PG_RELOAD_CONF();
```

## JIT compilation

JIT compilation is `on` by default:

```sql
SHOW jit;
-- on
```

```sql
WITH pi AS (SELECT RANDOM() x, RANDOM() y
            FROM GENERATE_SERIES(1, 10000000))
SELECT 4 * SUM(1 - FLOOR(x * x + y * y)) / COUNT(*) val
FROM pi;
```

```sql
EXPLAIN (ANALYZE, TIMING OFF)
WITH pi AS (SELECT RANDOM() x, RANDOM() y
            FROM GENERATE_SERIES(1, 10000000))
SELECT 4 * SUM(1 - FLOOR(x * x + y * y)) / COUNT(*) val
FROM pi;
```

```console
Aggregate  (cost=525000.00..525000.02 rows=1 width=8) (actual rows=1 loops=1)
  CTE pi
    ->  Function Scan on generate_series  (cost=0.00..150000.00 rows=10000000 width=16) (actual rows=10000000 loops=1)
  ->  CTE Scan on pi  (cost=0.00..200000.00 rows=10000000 width=16) (actual rows=10000000 loops=1)
Planning Time: 0.131 ms
JIT:
  Functions: 6
"  Options: Inlining true, Optimization true, Expressions true, Deforming true"
Execution Time: 4849.764 ms
```

Turn off JIT and execute above command again:

```sql
SET jit = off;
```

```console
Aggregate  (cost=525000.00..525000.02 rows=1 width=8) (actual rows=1 loops=1)
  CTE pi
    ->  Function Scan on generate_series  (cost=0.00..150000.00 rows=10000000 width=16) (actual rows=10000000 loops=1)
  ->  CTE Scan on pi  (cost=0.00..200000.00 rows=10000000 width=16) (actual rows=10000000 loops=1)
Planning Time: 0.183 ms
Execution Time: 5982.210 ms
```

In the future to simplify execution plans we will turn JIT off:

```sql
SET jit = off;
```

## Prepared operators

Defining:

```sql
PREPARE model(varchar) AS
    SELECT model
    FROM aircrafts
    WHERE aircraft_code = $1;
```

Calling

```sql
EXECUTE model('773');
```

All prepared operators can be accessed via `pg_prepared_statements` view:

```sql
SELECT *
FROM pg_prepared_statements;
```

| name  | statement                                                                                       | prepare_time                      | parameter_types       | result_types | from_sql | generic_plans | custom_plans |
| :---- | :---------------------------------------------------------------------------------------------- | :-------------------------------- | :-------------------- | :----------- | :------- | :------------ | :----------- |
| S_1   |                                                                                                 | 2024-08-03 10:13:03.483718 +00:00 | {}                    | null         | false    | 12            | 0            |
| model | PREPARE model\(varchar\) AS<br/> SELECT model<br/> FROM aircrafts<br/> WHERE aircraft_code = $1 | 2024-08-03 10:12:30.390164 +00:00 | {"character varying"} | {text}       | true     | 0             | 3            |

## Cursors

Begin transaction:

```sql
BEGIN;
```

Declaring cursors opens it automatically:

```sql
DECLARE c CURSOR FOR SELECT *
                     FROM aircrafts;
```

Calling `FETCH`, multiple times consequently, read rows:

```sql
FETCH c;
```

Close cursor:

```sql
CLOSE c;
```

Commit transaction:

```sql
COMMIT;
```

## More about planning

- Planning process
- Cardinality estimation
- Cost estimation
- Choosing best plan
