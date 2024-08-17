# SQL Server isolation levels

| Isolation level  | Dirty read | Non-repeatable read | Phantom read | Serialization anomaly and other anomalies |
| ---------------- | ---------- | ------------------- | ------------ | ----------------------------------------- |
| Read uncommitted | Yes        | Yes                 | Yes          | Yes                                       |
| Read committed   | No         | Yes                 | Yes          | Yes                                       |
| Repeatable read  | No         | No                  | Yes          | Yes                                       |
| Snapshot         | No         | No                  | No           | Yes                                       |
| Serializable     | No         | No                  | No           | No                                        |

## Table of contents

- [SQL Server isolation levels](#sql-server-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
  - [Common commands](#common-commands)
    - [Setting isolation level](#setting-isolation-level)
    - [Displaying current isolation level](#displaying-current-isolation-level)
    - [Committing and rolling back transaction](#committing-and-rolling-back-transaction)
    - [Adding delay](#adding-delay)
  - [Timeout](#timeout)
  - [Read phenomena](#read-phenomena)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete)
  - [Read committed](#read-committed)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete-1)
  - [Repeatable read](#repeatable-read)
    - [`UPDATE`, `DELETE`](#update-delete)
    - [`INSERT`](#insert)
  - [Serializable](#serializable)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete-2)

## Running

Run [`docker-compose.yaml`](docker-compose.yaml) file:

```bash
docker-compose up -d
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

## Timeout

It's possible to specify timeout explicitly using [↑ `SET LOCK_TIMEOUT`](https://learn.microsoft.com/ru-ru/sql/t-sql/statements/set-lock-timeout-transact-sql) to avoid the hang:

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SET LOCK_TIMEOUT 5000; -- 5 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

## Read phenomena

### `UPDATE`, `INSERT`, `DELETE`

On `UPDATE` T2 outputs `200` as Bob's balance — dirty read.

On `INSERT` and `DELETE` T2 outputs different number of rows — also dirty read.

To avoid dirty read use `READ COMMITTED` for T2. In this case T2 blocks until you commit/rollback T1 or until you cancel T2.

```sql
-- T1
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';

-- INSERT INTO simple_bank.accounts(name, balance)
-- VALUES ('Alex', 100);

-- DELETE
-- FROM simple_bank.accounts
-- WHERE name = 'Alex';

WAITFOR DELAY '00:00:10'; -- 10 seconds

ROLLBACK; -- Or COMMIT;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

## Read committed

### `UPDATE`, `INSERT`, `DELETE`

On `UPDATE` T1 outputs `100` as Bob's balance at the first time and `200` the second time — non-repeatable read.

On `INSERT` and `DELETE` T1 sees different number of rows — phantom read.

To avoid non-repeatable read use `REPEATABLE READ` isolation level for T1. In this case T2 will block until T1 finishes. And T1 will print `100` both times.

To avoid phantom read on `DELETE` use `REPEATABLE READ` isolation level for T1.

To avoid phantom read on `INSERT` use `SERIALIZABLE` isolation level for T1.

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';

-- INSERT INTO simple_bank.accounts(name, balance)
-- VALUES ('Alex', 100);

-- DELETE
-- FROM simple_bank.accounts
-- WHERE name = 'Alex';

COMMIT;
```

On `UPDATE` T2 blocks until T1 commits. The final balance of Bob is `300`:

```sql
-- T1
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = balance + 100
WHERE name = 'Bob';

WAITFOR DELAY '00:00:10'; -- 10 seconds

COMMIT;
```

```sql
-- T2
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = balance + 100
WHERE name = 'Bob';

COMMIT;
```

By replacing both conditions, or by replacing just T2's condition,:

```sql
WHERE name = 'Bob';
```

with:

```sql
WHERE name = 'Bob'
  AND balance = 100;
```

we will get `200` as Bob's balance when both transactions commit.

## Repeatable read

### `UPDATE`, `DELETE`

T1 outputs the same result both times while T2 blocks until T1 commits:

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';

-- DELETE
-- FROM simple_bank.accounts
-- WHERE name = 'Alex';

COMMIT;
```

### `INSERT`

On `INSERT` `REPEATABLE READ` does not prevent phantom read, so you will see different results in two `SELECT`s:

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION;

INSERT INTO simple_bank.accounts(name, balance)
VALUES ('Alex', 100);

COMMIT;
```

## Serializable

### `UPDATE`, `INSERT`, `DELETE`

Both `SELECT`s in T1 will the same result sets for `UPDATE`, `INSERT` and `DELETE`. T2 blocks until T1 commits:

```sql
-- T1
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

BEGIN TRANSACTION;

SELECT *
FROM simple_bank.accounts;

WAITFOR DELAY '00:00:10'; -- 10 seconds

SELECT *
FROM simple_bank.accounts;

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION;

UPDATE simple_bank.accounts
SET balance = 200
WHERE name = 'Bob';

-- INSERT INTO simple_bank.accounts(name, balance)
-- VALUES ('Alex', 100);

-- DELETE
-- FROM simple_bank.accounts
-- WHERE name = 'Alex';

COMMIT;
```
