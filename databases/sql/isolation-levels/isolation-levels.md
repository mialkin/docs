# Isolation levels and read anomalies

## Table of contents

- [Isolation levels and read anomalies](#isolation-levels-and-read-anomalies)
  - [Table of contents](#table-of-contents)
  - [Isolation levels](#isolation-levels)
  - [Read anomalies](#read-anomalies)
    - [Lost update](#lost-update)
    - [Dirty reads](#dirty-reads)
    - [Non-repeatable reads](#non-repeatable-reads)
    - [Phantom reads](#phantom-reads)
    - [Serialization anomaly](#serialization-anomaly)

## Isolation levels

An **isolation level** is a particular locking strategy employed in the database system to avoid [read anomalies](#read-anomalies) also called _phenomena_.

The American National Standards Institute, ANSI, defines four isolation levels:

- Read uncommitted
- Read committed
- Repeatable read
- Serializable

| Isolation level  | Lost update | Dirty read | Non-repeatable read | Phantom read | Serialization anomaly and other anomalies |
| ---------------- | ----------- | ---------- | ------------------- | ------------ | ----------------------------------------- |
| Read uncommitted | —           | Yes        | Yes                 | Yes          | Yes                                       |
| Read committed   | —           | —          | Yes                 | Yes          | Yes                                       |
| Repeatable read  | —           | —          | —                   | Yes          | Yes                                       |
| Serializable     | —           | —          | —                   | —            | —                                         |

Although higher isolation levels provide better data consistency, this consistency can be costly in terms of the parallel access provided to users. As isolation level increases, so does the chance that the locking strategy used will create problems with parallel access of data.

## Read anomalies

### Lost update

The **lost update** is an anomaly that occurs when two transactions read one and the same table row, then one of the transactions updates this row, and finally the other transaction updates the same row without taking into account any changes made by the first transaction.

Suppose that two transactions are going to increase the balance of one and the same account by $100. The first transaction reads the current value $1000, then the second transaction reads the same value. The first transaction increases the balance, making it $1100, and writes the new value into the database. The second transaction does the same: it gets $1100 after increasing the balance and writes this value. As a result, the customer loses $100.

Lost updates are forbidden by the standard at all isolation levels.

### Dirty reads

A **dirty read** is an anomaly that occurs when a transaction reads uncommitted changes made by another transaction.

```text
User 1 modifies a row.
User 2 reads the same row before User 1 commits.
User 1 performs a rollback.
User 2 has read row's values that have never really existed in the database.
```

### Non-repeatable reads

A **non-repeatable read** is an anomaly that occurs when a transaction reads one and the same row twice, whereas another transaction updates or deletes this row between these reads and commits the change. As a result, the first transaction gets different results.

```text
User 1 reads a row, but does not commit.
User 2 modifies or deletes the same row and then commits.
User 1 rereads the row and finds it has been changed.
```

### Phantom reads

A **phantom read** anomaly occurs when one and the same transaction executes two identical queries returning a set of rows that satisfy a particular condition, while another transaction adds some other rows satisfying this condition and commits the changes in the time interval between these queries. As a result, the first transaction gets two different sets of rows.

```text
User 1 uses a search condition to read a set of rows, but does not commit.
User 2 inserts one or more rows that satisfy this search condition, then commits.
User 1 rereads the rows using the search condition and discovers rows that were not present before.
```

### Serialization anomaly

[↑ Transaction Isolation Levels With PostgreSQL as an example](https://mkdev.me/posts/transaction-isolation-levels-with-postgresql-as-an-example).
