# PostgreSQL isolation levels

In PostgreSQL, you can request any of the four standard transaction isolation levels, but internally only three distinct isolation levels are implemented, i.e., PostgreSQL's read uncommitted mode behaves like read committed. This is because it is the only sensible way to map the standard isolation levels to PostgreSQL's multiversion concurrency control architecture.

The table also shows that PostgreSQL's repeatable read implementation does not allow phantom reads. This is acceptable under the SQL standard because the standard specifies which anomalies must not occur at certain isolation levels; higher guarantees are acceptable. The behavior of the available isolation levels is detailed in the following subsections.

| Isolation level  | Dirty read             | Non-repeatable read | Phantom read           | Serialization anomaly |
| ---------------- | ---------------------- | ------------------- | ---------------------- | --------------------- |
| Read uncommitted | Allowed, but not in PG | Possible            | Possible               | Possible              |
| Read committed   | Not possible           | Possible            | Possible               | Possible              |
| Repeatable read  | Not possible           | Not possible        | Allowed, but not in PG | Possible              |
| Serializable     | Not possible           | Not possible        | Not possible           | Not possible          |

[↑ 13.2. Transaction Isolation](https://www.postgresql.org/docs/16/transaction-iso.html).

## Table of contents

- [PostgreSQL isolation levels](#postgresql-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)
  - [Common commands](#common-commands)
    - [Setting isolation level](#setting-isolation-level)
    - [Displaying current isolation level](#displaying-current-isolation-level)
    - [Committing and rolling back transaction](#committing-and-rolling-back-transaction)
    - [Adding delay](#adding-delay)
  - [Read phenomena](#read-phenomena)
    - [Dirty read](#dirty-read)
    - [Non-repeatable read](#non-repeatable-read)
      - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete)
  - [Repeatable read](#repeatable-read)
    - [`UPDATE`, `INSERT`, `DELETE`](#update-insert-delete-1)

## Running

Run [`docker-compose.yaml`](docker-compose.yaml) file:

```bash
docker-compose up --detach
```

Create some test data:

```sql
BEGIN TRANSACTION;

CREATE TABLE accounts
(
    name    text   NOT NULL UNIQUE,
    balance bigint NOT NULL
);

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
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
-- BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;
-- BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
-- BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### Displaying current isolation level

```sql
SHOW TRANSACTION ISOLATION LEVEL;
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

It's possible to delay execution of a command inside transaction using [↑ `pg_sleep`](https://www.postgresql.org/docs/16/functions-datetime.html#FUNCTIONS-DATETIME-DELAY) function:

```sql
SELECT CURRENT_TIMESTAMP;
SELECT pg_sleep(10); -- 10 seconds
SELECT CURRENT_TIMESTAMP;
```

## Read phenomena

### Dirty read

Dirty reads in PostgreSQL are forbidden by design.

Based on snapshots, PostgreSQL isolation differs from the requirements specified in the standard — in fact, it is even stricter.

Results of `UPDATE`, `INSERT` and `DELETE` operations in `T1` are not reflected in `T2`:

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

SELECT PG_SLEEP(10); -- 10 seconds

ROLLBACK; -- Or COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

SELECT *
FROM accounts;

COMMIT;
```

| name  | balance |
| :---- | :------ |
| Bob   | 100     |
| Alice | 100     |

### Non-repeatable read

#### `UPDATE`, `INSERT`, `DELETE`

On `UPDATE` T1 shows different values for Bob's balance — non-repeatable read.

On `INSERT` and `DELETE` T1 shows different number of rows — phantom read.

```sql
-- T1
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;

SELECT *
FROM simple_bank.accounts;

SELECT pg_sleep(10); -- 10 seconds

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

UPDATE accounts
SET balance = balance + 100
WHERE name = 'Bob';

SELECT PG_SLEEP(10);

COMMIT;
```

```sql
-- T2
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;

UPDATE accounts
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

### `UPDATE`, `INSERT`, `DELETE`

On `UPDATE` `REPEATABLE READ` prevents non-repeatable read — T1 reads `100` as Bob's balance both times.

On `INSERT` and `DELETE` `REPEATABLE READ` prevents phantom reads — T1 reads both times the same number of rows.

```sql
-- T1
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;

SELECT *
FROM simple_bank.accounts;

SELECT pg_sleep(10); -- 10 seconds

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
