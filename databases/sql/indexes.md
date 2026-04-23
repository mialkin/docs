# Covering, composite, partial index, cardinality, selectivity

## Table of contents

- [Covering, composite, partial index, cardinality, selectivity](#covering-composite-partial-index-cardinality-selectivity)
  - [Table of contents](#table-of-contents)
  - [Covering index](#covering-index)
  - [Composite index](#composite-index)
  - [Partial index](#partial-index)
  - [Cardinality](#cardinality)
  - [Selectivity](#selectivity)

## Covering index

A **covering index** contains the data to be searched through include, so that the SQL query can get the required data without reaching the basic table.

When the columns returned by `SELECT` are not many, consider building a covering index on these columns.

[↑ Index-Only Scans and Covering Indexes](https://www.postgresql.org/docs/current/indexes-index-only-scans.html).

## Composite index

The **composite index** is to create an index on the combination of multiple columns, these columns may contain all the columns of the query, or may not contain. When the column in the composite index contains all the columns in the query, the index is also a covering index. When the composite index does not contain all the data to be queried, the query needs to find the location information of the data through the non-clustered index, and then go to the basic table to find the data.

## Partial index

A partial index is an index built over a subset of a table; the subset is defined by a conditional expression (called the predicate of the partial index). The index contains entries only for those table rows that satisfy the predicate. Partial indexes are a specialized feature, but there are several situations in which they are useful.

## Cardinality

A **cardinality** is the the number of unique values in a column.

If cardinality is high, it means the values are very diverse. On the other hand, if the column has many repeated values, such as gender (with only "male" and "female"), the cardinality is low.

To measure the cardinality of a specific column in SQL, you can use the `COUNT` and `DISTINCT` functions:

$$Cardinality = COUNT(DISTINCT column\_name)$$

For example, if a table has 1,000,000 rows but the "Country" column only has 195 distinct values, it is considered low cardinality. This information tells the database engine that using a standard B-tree index might be less efficient than a bitmap index.

## Selectivity

A **selectivity** is an index's ability to reduce the number of rows returned by a query.

A highly selective index filters out a large portion of the data and therefore speeds up the search. When you query a field that has many distinct values (in other words, is very unique), the index selectivity will be high.

The [HyperLogLog](../../computer-science/algorithms/algorithms.md#hyperloglog) algorithm can be used for cardinality estimation.

To check index selectivity run this on the production database:

```sql
SELECT
    COUNT(*)                        AS TotalRows,
    COUNT(DISTINCT Sku)             AS DistinctSkus,
    CAST(COUNT(DISTINCT Sku) AS FLOAT) / COUNT(*) AS Selectivity
FROM Orders;
```

How to interpret the result:

- Selectivity close to 1.0 → high selectivity, index is very useful
- Selectivity close to 0.0 → low selectivity (e.g., a bit column), index may hurt more than help
