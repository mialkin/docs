# Postgres index types

PostgreSQL provides several [↑ index types](https://www.postgresql.org/docs/current/indexes-types.html): B-tree, Hash, GiST, SP-GiST, GIN, BRIN, and the extension bloom. Each index type uses a different algorithm that is best suited to different types of indexable clauses. By default, the `CREATE INDEX` command creates B-tree indexes, which fit the most common situations. The other index types are selected by writing the keyword `USING` followed by the index type name:

```sql
CREATE INDEX name ON table USING HASH (column);
```

## Table of contents

- [Postgres index types](#postgres-index-types)
  - [Table of contents](#table-of-contents)
  - [B-tree index](#b-tree-index)
  - [Hash index](#hash-index)
  - [Clustered index](#clustered-index)

## B-tree index

A [↑ **B-tree index**](https://www.postgresql.org/docs/current/indexes-types.html#INDEXES-TYPES-BTREE) can handle equality and range queries on data that can be sorted into some ordering. In particular, the PostgreSQL query planner will consider using a B-tree index whenever an indexed column is involved in a comparison using one of these operators:

```text
<   <=   =   >=   >
```

The optimizer can also use a B-tree index for queries involving the pattern matching operators `LIKE` and `~` if the pattern is a constant and is anchored to the beginning of the string — for example, `col LIKE 'foo%'` or `col ~ '^foo'`, but not `col LIKE '%bar'`.

B-tree indexes can also be used to retrieve data in sorted order. This is not always faster than a simple scan and sort, but it is often helpful.

Each tree node of B-tree index contains several elements, which consist of an index key and a pointer. Inner node elements reference nodes of the next level; leaf node elements reference heap tuples.

<img src="b-tree.jpeg" width="700px" alt="Schematic diagram of a B-tree"/>

For the search path to be unambiguously identified, all tree elements must be ordered. B-trees are designed for ordinal data types, whose values can be compared and sorted.

B-trees are balanced, which means that all leaf nodes of a tree are located at the same depth. Therefore, they guarantee equal search time for all values. They have plenty of branches, that is, each node contains many elements, often hundreds of them. As a result, B-tree depth is always small, even for very large tables.

In Postgres every table and index is stored as an array of pages of a fixed size — [↑ usually 8 kB](https://www.postgresql.org/docs/current/storage-page-layout.html), although a different page size can be selected when compiling the server. If, for example, and index page fits 100 index keys per page on average, then for the table containing 1 billion rows the tree height will be $log_{100} 1000000000 = 4.5$

Data in an index is sorted either in ascending or in descending order, both within each node and across all nodes of the same level. Peer nodes are bound into a bidirectional list, so it is possible to get an ordered set of data by simply scanning the list one way or the other, without having to start at the root each time.

We cannot say with absolute certainty what the letter B in the name "B-tree" stands for. Both _balanced_ and _bushy_ fit equally well. Surprisingly, you can often see it interpreted as _binary_, which is certainly incorrect.

[↑ B-Tree индекс и его производные в PostgreSQL](https://habr.com/ru/companies/quadcode/articles/696498/).

Index does not store row visibility information. That's why deletion of a row does not require index update. Index therefore may get bloated. [↑ Vacuum](https://www.postgresql.org/docs/current/routine-vacuuming.html) fixes index bloat.

## Hash index

A [↑ **hash index**](https://www.postgresql.org/docs/current/indexes-types.html#INDEXES-TYPES-HASH) stores a 32-bit hash code derived from the value of the indexed column. Hence, such indexes can only handle simple equality comparisons. The query planner will consider using a hash index whenever an indexed column is involved in a comparison using the equal operator:

```text
=
```

## Clustered index

PostgreSQL [↑ does not](https://stackoverflow.com/a/40951076/1833895) have direct implementation of [↑ clustered index](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described) like Microsoft SQL Server.

In PostgreSQL, there is a [↑ `CLUSTER`](https://www.postgresql.org/docs/current/sql-cluster.html) command which cluster a table according to an index.

Once you create your table primary key or any other index, you can execute the `CLUSTER` command by specifying that index name to achieve the physical order of the table data.

When a table is clustered, it is physically reordered based on the index information. Clustering is a one-time operation: when the table is subsequently updated, the changes are not clustered. That is, no attempt is made to store new or updated rows according to their index order.
