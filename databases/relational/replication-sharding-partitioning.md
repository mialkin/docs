# Replication, sharding, partitioning

## Replication

The primary goal of database replication is to enhance fault tolerance and availability by creating duplicate copies of the database on multiple servers.

In replication, data is copied from one database, master, to one or more other databases, replicas or slaves, in near real-time. Changes made to the master are propagated to the replicas.

Advantages:

- Fault tolerance: If one server fails, others can take over.
- Read scalability: Read queries can be distributed among replicas, reducing the load on the master.

## Sharding

Sharding, also known as horizontal partitioning, is used to distribute large datasets across multiple databases or servers to improve performance and scalability.

In sharding, the dataset is divided into smaller, independent subsets, shards, and each shard is stored on a separate server. Each server manages its own subset of data.

Advantages:

- Scalability: As data grows, additional servers can be added to handle increased load.
- Improved performance: Queries can be parallelized across shards, enhancing overall query performance.

Challenges:

- Complex to implement: Sharding often requires careful planning and design to distribute data effectively.
- Data integrity: Ensuring consistency across shards can be challenging.

## Partitioning

Partitioning, also known as vertical partitioning, involves dividing a table into smaller, more manageable pieces based on columns.

Different columns of a table are separated into partitions, and each partition can be stored on separate storage devices or servers. Each partition may contain a subset of the rows but all columns.

Advantages:

- Improved query performance: Queries that access only specific columns can be more efficient.
- Easier maintenance: Smaller partitions can be managed more easily than large tables.

Challenges:

- Limited scalability: While partitioning can improve performance, it may not be as scalable as sharding for handling large datasets.

In summary, replication is about creating copies of the entire database for fault tolerance, sharding involves distributing the dataset horizontally across multiple servers for scalability, and partitioning divides a table vertically based on columns to improve query performance and maintenance. Often, these techniques can be used in combination to achieve the desired level of performance, scalability, and fault tolerance for a given database system.
