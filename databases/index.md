# Covering, composite, clustered index, cardinality, index selectivity

## Table of contents

- [Covering, composite, clustered index, cardinality, index selectivity](#covering-composite-clustered-index-cardinality-index-selectivity)
  - [Table of contents](#table-of-contents)
  - [Covering index](#covering-index)
  - [Composite index](#composite-index)
  - [Clustered index](#clustered-index)
  - [Partial index](#partial-index)
  - [Cardinality](#cardinality)
  - [Index selectivity](#index-selectivity)

## Covering index

A **covering index** contains the data to be searched through include, so that the SQL query can get the required data without reaching the basic table.

When the columns returned by `SELECT` are not many, consider building a covering index on these columns.

[↑ Index-Only Scans and Covering Indexes](https://www.postgresql.org/docs/current/indexes-index-only-scans.html).

## Composite index

The **composite index** is to create an index on the combination of multiple columns, these columns may contain all the columns of the query, or may not contain. When the column in the composite index contains all the columns in the query, the index is also a covering index. When the composite index does not contain all the data to be queried, the query needs to find the location information of the data through the non-clustered index, and then go to the basic table to find the data.

## Clustered index

A **clustered index** means telling the database to store the close values actually close to one another on the disk.

[↑ About clustered index in Postgres](https://stackoverflow.com/questions/4796548/about-clustered-index-in-postgres).

## Partial index

A partial index is an index built over a subset of a table; the subset is defined by a conditional expression (called the predicate of the partial index). The index contains entries only for those table rows that satisfy the predicate. Partial indexes are a specialized feature, but there are several situations in which they are useful.

## Cardinality

A **cardinality** is the the number of unique values in particular column.

## Index selectivity

An **index selectivity** is a fraction of rows in a table having the same value for the indexed key.

An index selectivity is calculated using the [cardinality](#database-cardinality):

```text
index selectivity = cardinality / (number of rows) * 100%
```

A lower selectivity value is better: it means fewer rows to scan and filter. Oddly, though, we say an index is "highly" selective if it has a low selectivity value.  When a database plans a query, it'll look in its index statistics and estimate how many rows the query will match. Databases can have simple statistics where they simply keep track of an estimate of cardinality, or they can have so-called "histogram" statistics where they track the frequency of ranges of values.

Accessing a table through an index adds some overhead and query planner may decide that it will be quicker just to scan the whole table rather than using the index.
