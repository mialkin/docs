# Covering, composite, clustered indexes

## Covering index

A **covering index** contains the data to be searched through include, so that the SQL query can get the required data without reaching the basic table.

When the columns returned by `SELECT` are not many, consider building a covering index on these columns.

[↑ Index-Only Scans and Covering Indexes](https://www.postgresql.org/docs/current/indexes-index-only-scans.html).

## Composite index

The **composite index** is to create an index on the combination of multiple columns, these columns may contain all the columns of the query, or may not contain. When the column in the composite index contains all the columns in the query, the index is also a covering index. When the composite index does not contain all the data to be queried, the query needs to find the location information of the data through the non-clustered index, and then go to the basic table to find the data.

## Clustered index

A **clustered index** means telling the database to store the close values actually close to one another on the disk.

[↑ About clustered index in Postgres](https://stackoverflow.com/questions/4796548/about-clustered-index-in-postgres).
