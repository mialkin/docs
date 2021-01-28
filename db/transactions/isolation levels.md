# Isolation levels

An **isolation level** represents a particular locking strategy employed in the database system to avoid _read phenomena_.

## Read phenomena

### Dirty reads

A **dirty read** is a phenomenon that occurs when a transaction is allowed to read data from a row that has been modified by another running transaction and not yet committed.

```text
User 1 modifies a row. User 2 reads the same row before User 1 commits. User 1 performs a rollback.User 2 has read row's values that have never really existed in the database.
```

### Non-repeatable reads

A **non-repeatable read** is a phenomenon that occurs when, during the course of a transaction, a row is retrieved twice and the values within the row differ between reads.

```text
User 1 reads a row, but does not commit. User 2 modifies or deletes the same row and then commits. User 1 rereads the row and finds it has been changed/deleted.
```

## List of isolation levels

Isolation levels represent the database system's ability to prevent these read phenomena. The American National Standards Institute (ANSI) defines four isolation levels:

- Read uncommitted (0)
- Read committed (1)
- Repeatable read (2)
- Serializable (3)

See 1995 article: [A Critique of ANSI SQL Isolation Levels](tr-95-51.pdf).

| Isolation level      | Phantom reads | Non-repeatable reads | Dirty reads |
| -------------------- | ------------- | -------------------- | ----------- |
| Read uncommitted (0) | +             | +                    | +           |
| Read committed (1))  | +             | +                    | -           |
| Repeatable read (2)  | +             | -                    | -           |
| Serializable (3)     | -             | -                    | -           |

Although higher isolation levels provide better data consistency, this consistency can be costly in terms of the parallel access provided to users. As isolation level increases, so does the chance that the locking strategy used will create problems with parallel access.
