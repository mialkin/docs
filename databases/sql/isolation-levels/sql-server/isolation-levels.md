# SQL Server isolation levels

| Isolation level  | Lost update | Dirty read | Non-repeatable read | Phantom read | Serialization anomaly and other anomalies |
| ---------------- | ----------- | ---------- | ------------------- | ------------ | ----------------------------------------- |
| Read uncommitted | —           | Yes        | Yes                 | Yes          | Yes                                       |
| Read committed   | —           | —          | Yes                 | Yes          | Yes                                       |
| Repeatable read  | —           | —          | —                   | Yes          | Yes                                       |
| Snapshot         | —           | —          | —                   | —            | Yes                                       |
| Serializable     | —           | —          | —                   | —            | —                                         |

## Table of contents

- [SQL Server isolation levels](#sql-server-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
  - [Common commands](#common-commands)
    - [Setting isolation level](#setting-isolation-level)
    - [Displaying current isolation level](#displaying-current-isolation-level)
    - [Committing and rolling back transaction](#committing-and-rolling-back-transaction)
    - [Adding delay](#adding-delay)
  - [Read phenomena](#read-phenomena)
    - [Lost update](#lost-update)
    - [Dirty read](#dirty-read)
    - [Non-repeatable read](#non-repeatable-read)
    - [Phantom read](#phantom-read)
    - [Serialization anomaly](#serialization-anomaly)

## Running

Run [`docker-compose.yaml`](docker-compose.yaml) file:

```bash
docker compose up --detach
```

Create some test data:

```sql
BEGIN TRANSACTION;

CREATE TABLE accounts
(
    name    VARCHAR(20) NOT NULL
        CONSTRAINT accounts_pk
            UNIQUE,
    balance bigint        NOT NULL
)

INSERT INTO accounts (name, balance)
VALUES ('Bob', 100),
       ('Alice', 100);

COMMIT;
```

```sql
SELECT *
FROM accounts;
```

| name  | balance |
| :---- | :------ |
| Bob   | 100     |
| Alice | 100     |

## Common commands

### Setting isolation level

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
-- SET TRANSACTION ISOLATION LEVEL READ COMMITTED
-- SET TRANSACTION ISOLATION LEVEL REPEATABLE READ
-- SET TRANSACTION ISOLATION LEVEL SNAPSHOT
-- SET TRANSACTION ISOLATION LEVEL SERIALIZABLE
```

### Displaying current isolation level

```sql
CREATE FUNCTION display_isolation_level()
    RETURNS CHAR(20)
BEGIN
    DECLARE @isolation_level CHAR(20);
    SET @isolation_level = (SELECT CASE transaction_isolation_level
                                       WHEN 0 THEN 'Unspecified'
                                       WHEN 1 THEN 'ReadUncommitted'
                                       WHEN 2 THEN 'ReadCommitted'
                                       WHEN 3 THEN 'Repeatable'
                                       WHEN 4 THEN 'Serializable'
                                       WHEN 5 THEN 'Snapshot' END AS transaction_isolation_level
                            FROM sys.dm_exec_sessions
                            WHERE session_id = @@SPID);
    RETURN @isolation_level
END
```

```sql
SELECT dbo.display_isolation_level() as current_isolation_level;
```

### Committing and rolling back transaction

```sql
BEGIN TRANSACTION;

COMMIT;
```

```sql
BEGIN TRANSACTION;

ROLLBACK;
```

### Adding delay

It's possible to delay execution of a command inside transaction using [↑ `WAITFOR`](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/waitfor-transact-sql) keyword:

```sql
WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM accounts;
```

It's possible to specify timeout explicitly using [↑ `SET LOCK_TIMEOUT`](https://learn.microsoft.com/ru-ru/sql/t-sql/statements/set-lock-timeout-transact-sql) to avoid the hang:

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SET LOCK_TIMEOUT 5000; -- 5 seconds

SELECT *
FROM accounts;

COMMIT;
```

## Read phenomena

### Lost update

```sql
-- T1
BEGIN TRANSACTION;

UPDATE accounts
SET balance = balance + 100
WHERE name = 'Bob';

WAITFOR DELAY '00:00:10'; -- 10 seconds

COMMIT;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN TRANSACTION;

UPDATE accounts
SET balance = balance + 100
WHERE name = 'Bob';

COMMIT;

SELECT *
FROM accounts;
```

| name  | balance |
| :---- | :------ |
| Bob   | 300     |
| Alice | 100     |

In the example above `T2` blocks until `T1` commits.

By replacing `T2`'s `WHERE` condition with

```sql
WHERE name = 'Bob'
  AND balance = 100;
```

you will get `200`:

| name  | balance |
| :---- | :------ |
| Bob   | 200     |
| Alice | 100     |

### Dirty read

Results of `UPDATE`, `INSERT` and `DELETE` uncommitted operations in `T1` are all reflected in `T2` — that's a dirty read:

```sql
-- T1
BEGIN TRANSACTION;

UPDATE accounts
SET balance = 200
WHERE name = 'Bob';

INSERT INTO accounts(name, balance)
VALUES ('Jacob', 100);

DELETE
FROM accounts
WHERE name = 'Alice';

WAITFOR DELAY '00:00:10'; -- 10 seconds

ROLLBACK; -- Or COMMIT;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
BEGIN TRANSACTION;

SELECT *
FROM accounts;

COMMIT;
```

| name  | balance |
| :---- | :------ |
| Bob   | 200     |
| Jacob | 100     |

By changing isolation level from `READ UNCOMMITTED` to `READ COMMITTED`, in previous example, the dirty read is not observed anymore — `T2` blocks until `T1` rolls back. `T2` outputs unmodified data after `T1` rolls back.

### Non-repeatable read

Below the results of committed `UPDATE` and `DELETE` operations in `T2` are all reflected in `T1` — that's a non-repeatable read.

The first time `T1` reads:

| name  | balance |
| :---- | :------ |
| Bob   | 100     |
| Alice | 100     |

The second time `T1` reads:

| name | balance |
| :--- | :------ |
| Bob  | 200     |

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM accounts;

COMMIT; -- Or ROLLBACK;
```

```sql
-- T2
BEGIN TRANSACTION;

UPDATE accounts
SET balance = 200
WHERE name = 'Bob';

DELETE
FROM accounts
WHERE name = 'Alice';

COMMIT;
```

By changing isolation level from `READ COMMITTED` to `REPEATABLE READ`, in previous example, the non-repeatable read is not observed anymore — `T2` blocks until `T1` commits. `T1` outputs unmodified data both times.

### Phantom read

Below the result of committed `INSERT` operation in `T2` is reflected in `T1` — that's a phantom read.

The first time `T1` reads:

| name  | balance |
| :---- | :------ |
| Bob   | 100     |
| Alice | 100     |

The second time `T1` reads:

| name  | balance |
| :---- | :------ |
| Bob   | 100     |
| Alice | 100     |
| Jacob | 100     |

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN TRANSACTION;

SELECT *
FROM accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM accounts;

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION;

INSERT INTO accounts(name, balance)
VALUES ('Jacob', 100);

COMMIT;
```

The first select in `T1` outputs:

| name  | balance |
| :---- | :------ |
| Bob   | 100     |
| Alice | 100     |

The second select in `T1` outputs:

| name  | balance |
| :---- | :------ |
| Bob   | 100     |
| Alice | 100     |
| Jacob | 100     |

By changing isolation level from `REPEATABLE READ` to `SNAPSHOT`, in previous example, the non-repeatable read is not observed anymore — `T2` executes without blocking, and `T1` outputs unmodified data both times.

By changing isolation level from `REPEATABLE READ` to `SERIALIZABLE`, in previous example, the non-repeatable read is not observed anymore — `T2` blocks until `T1` commits. `T1` outputs unmodified data both times.

### Serialization anomaly

Except when a database is being recovered, SNAPSHOT transactions [↑ do not request locks](https://learn.microsoft.com/en-us/sql/t-sql/statements/set-transaction-isolation-level-transact-sql) when reading data. SNAPSHOT transactions reading data do not block other transactions from writing data. Transactions writing data do not block SNAPSHOT transactions from reading data.
