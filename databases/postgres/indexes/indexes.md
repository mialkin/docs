# Postgres indexes

## Table of contents

- [Postgres indexes](#postgres-indexes)
  - [Table of contents](#table-of-contents)
  - [Hash](#hash)
  - [B-tree](#b-tree)
  - [Clustered index](#clustered-index)

## Hash

A **hash index** provides the ability to quickly find a tuple ID (TID) by a particular index key. Roughly speaking, it is simply a hash table stored on disk. The only operation supported by a hash index is search by the equality condition.

## B-tree

A **B-tree** is a data structure that enables you to quickly find the required element in leaf nodes of the tree by going down from its root.

<img src="b-tree.jpeg" width="700px" alt="Schematic diagram of a B-tree"/>

Each tree node contains several elements, which consist of an index key and a pointer. Inner node elements reference nodes of the next level; leaf node elements reference heap tuples.

For the search path to be unambiguously identified, all tree elements must be ordered. B-trees are designed for ordinal data types, whose values can be compared and sorted.

B-trees have the following important properties:

- They are balanced, which means that all leaf nodes of a tree are located at the same depth. Therefore, they guarantee equal search time for all values.
- They have plenty of branches, that is, each node contains many elements, often hundreds of them (the illustration shows three-element nodes solely for clarity). As a result, B-tree depth is always small, even for very large tables.
- Data in an index is sorted either in ascending or in descending order, both within each node and across all nodes of the same level. Peer nodes are bound into a bidirectional list, so it is possible to get an ordered set of data by simply scanning the list one way or the other, without having to start at the root each time.

We cannot say with absolute certainty what the letter B in the name of this structure stands for. Both _balanced_ and _bushy_ fit equally well. Surprisingly, you can often see it interpreted as _binary_, which is certainly incorrect.

[↑ B-Tree индекс и его производные в PostgreSQL](https://habr.com/ru/companies/quadcode/articles/696498/).

## Clustered index

PostgreSQL [↑ does not](https://stackoverflow.com/a/40951076/1833895) have direct implementation of [↑ clustered index](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described) like Microsoft SQL Server.

In PostgreSQL, there is a [↑ `CLUSTER`](https://www.postgresql.org/docs/current/sql-cluster.html) command which cluster a table according to an index.

Once you create your table primary key or any other index, you can execute the `CLUSTER` command by specifying that index name to achieve the physical order of the table data.

When a table is clustered, it is physically reordered based on the index information. Clustering is a one-time operation: when the table is subsequently updated, the changes are not clustered. That is, no attempt is made to store new or updated rows according to their index order.
