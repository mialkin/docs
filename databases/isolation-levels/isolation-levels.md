# Isolation levels and read phenomena

## Table of contents

- [Isolation levels and read phenomena](#isolation-levels-and-read-phenomena)
  - [Table of contents](#table-of-contents)
  - [Isolation levels](#isolation-levels)
  - [Read phenomena](#read-phenomena)
    - [Dirty reads](#dirty-reads)
    - [Non-repeatable reads](#non-repeatable-reads)
    - [Phantom reads](#phantom-reads)
    - [Overall picture](#overall-picture)

## Isolation levels

An **isolation level** is a particular locking strategy employed in the database system to avoid [read phenomena](#read-phenomena) also called _anomalies_.

The American National Standards Institute, ANSI, defines four isolation levels:

- Read uncommitted
- Read committed
- Repeatable read
- Serializable

| Isolation level  | Dirty read | Non-repeatable read | Phantom read | Serialization anomaly |
| ---------------- | ---------- | ------------------- | ------------ | --------------------- |
| Read uncommitted | Yes        | Yes                 | Yes          | Yes                   |
| Read committed   | No         | Yes                 | Yes          | Yes                   |
| Repeatable read  | No         | No                  | Yes          | Yes                   |
| Snapshot         | No         | No                  | No           | Yes                   |
| Serializable     | No         | No                  | No           | No                    |

Although higher isolation levels provide better data consistency, this consistency can be costly in terms of the parallel access provided to users. As isolation level increases, so does the chance that the locking strategy used will create problems with parallel access of data.

## Read phenomena

### Dirty reads

A **dirty read** is a phenomenon that occurs when a transaction is allowed to read data from a row that has been modified by another running transaction and not yet committed.

```text
User 1 modifies a row.
User 2 reads the same row before User 1 commits.
User 1 performs a rollback.
User 2 has read row's values that have never really existed in the database.
```

### Non-repeatable reads

A **non-repeatable read** is a phenomenon that occurs when, during the course of a transaction, a row is retrieved twice and the values within the row differ between reads.

```text
User 1 reads a row, but does not commit.
User 2 modifies or deletes the same row and then commits.
User 1 rereads the row and finds it has been changed.
```

### Phantom reads

A **phantom read** is a phenomenon that occurs when, in the course of a transaction, new rows are added or removed by another transaction to the records being read.

```text
User 1 uses a search condition to read a set of rows, but does not commit.
User 2 inserts one or more rows that satisfy this search condition, then commits.
User 1 rereads the rows using the search condition and discovers rows that were not present before.
```

### Overall picture

**Dirty reads** — read _uncommitted_ data from another transaction.

**Non-repeatable reads** — read _committed_ data from an `UPDATE` query from another transaction.

**Phantom reads** — read _committed_ data from an `INSERT` or `DELETE` query from another transaction.

[↑ What is the difference between Non-Repeatable Read and Phantom Read?](https://stackoverflow.com/questions/11043712/what-is-the-difference-between-non-repeatable-read-and-phantom-read)
