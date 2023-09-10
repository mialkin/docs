# Isolation levels and read phenomena

- [Isolation levels and read phenomena](#isolation-levels-and-read-phenomena)
  - [Read phenomena](#read-phenomena)
    - [Dirty reads](#dirty-reads)
    - [Non-repeatable reads](#non-repeatable-reads)
    - [Phantom reads](#phantom-reads)
    - [Overall picture](#overall-picture)
  - [Isolation levels](#isolation-levels)
    - [Read uncommitted](#read-uncommitted)
    - [Read committed](#read-committed)
    - [Repeatable read](#repeatable-read)
    - [Serializable](#serializable)
  - [DataGrip](#datagrip)
  - [Isolation levels in MySQL](#isolation-levels-in-mysql)
    - [Run MySQL](#run-mysql)
    - [Create `simple_bank` schema](#create-simple_bank-schema)
    - [Insert data](#insert-data)
    - [Get transaction isolation level of the current session](#get-transaction-isolation-level-of-the-current-session)
    - [Change isolation level](#change-isolation-level)
    - [Read uncommitted isolation level](#read-uncommitted-isolation-level)
    - [Read committed isolation level](#read-committed-isolation-level)

An **isolation level** represents a particular locking strategy employed in the database system to avoid *read phenomena*.

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

**Dirty reads** — read *uncommitted* data from another transaction.

**Non-repeatable reads** — read *committed* data from an `UPDATE` query from another transaction.

**Phantom reads** — read *committed* data from an `INSERT` or `DELETE` query from another transaction.

[↑ What is the difference between Non-Repeatable Read and Phantom Read?](https://stackoverflow.com/questions/11043712/what-is-the-difference-between-non-repeatable-read-and-phantom-read)

## Isolation levels

The American National Standards Institute (ANSI) defines four isolation levels:

- Read uncommitted
- Read committed
- Repeatable read
- Serializable

See 1995 article: [A Critique of ANSI SQL Isolation Levels](files/tr-95-51.pdf).

The plus sign `+` reads as "phenomenon can happen":

| Isolation level  | Dirty reads | Non-repeatable reads | Phantom reads |
| ---------------- | ----------- | -------------------- | ------------- |
| Read uncommitted | +           | +                    | +             |
| Read committed   | -           | +                    | +             |
| Repeatable read  | -           | -                    | +             |
| Serializable     | -           | -                    | -             |

Any phenomena that have been prevented at lower isolation level won't have a chance to occur at higher level.

Although higher isolation levels provide better data consistency, this consistency can be costly in terms of the parallel access provided to users. As isolation level increases, so does the chance that the locking strategy used will create problems with parallel access of data.

### Read uncommitted

Locks are obtained on modifications to the database and held until end of transaction (EOT). Reading from the database does not involve any locking.

### Read committed

Locks are acquired for reading and modifying the database. Locks are released after reading but locks on modified objects are held until EOT.

### Repeatable read

Locks are obtained for reading and modifying the database. Locks on all modified objects are held until EOT. Locks obtained for reading data are held until EOT. Locks on non-modified access structures (such as indexes and hashing structures) are released after reading.

Уровень, при котором читающая транзакция «не видит» изменения данных, которые были ею ранее прочитаны. При этом никакая другая транзакция не может изменять данные, читаемые текущей транзакцией, пока та не окончена.

### Serializable

All data read or modified is locked until EOT. All access structures that are modified are locked until EOT. Access structures used by the query are locked until EOT.

## DataGrip

Selection of an isolation level in DataGrip:

<img src="files/datagrip.png" width="350" />

## Isolation levels in MySQL

Following section contains examples from [↑ Deeply understand Isolation levels and Read phenomena in MySQL & PostgreSQL](https://dev.to/techschoolguru/understand-isolation-levels-read-phenomena-in-mysql-postgres-c2e) article.

### Run MySQL

Save this as `mysql-docker-compose.yml` file:

```yaml
# Use root/example as user/password credentials
version: "3.1"

services:
  mysql:
    image: mysql
    container_name: mysql
    # NOTE: use of "mysql_native_password" is not recommended: https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password
    # (this is just an example, not intended to be a production configuration)
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: example

  mysql-adminer:
    image: adminer
    container_name: adminer
    restart: always
    ports:
      - 8080:8080
```

Run container:

```bash
docker-compose -f mysql-docker-compose.yml up
watch docker ps -a
```

Connect to MySQL:

```bash
docker exec -it mysql mysql -uroot -p
```

A shortcut to clear console after executing a command: `Ctrl` + `L`.

### Create `simple_bank` schema

```sql
CREATE SCHEMA simple_bank;

USE simple_bank;

CREATE TABLE `accounts` (
  `id` bigint PRIMARY KEY AUTO_INCREMENT,
  `owner` varchar(255) NOT NULL,
  `balance` bigint NOT NULL,
  `currency` varchar(255) NOT NULL,
  `created_at` datetime NOT NULL DEFAULT (now())
);

CREATE TABLE `entries` (
  `id` bigint PRIMARY KEY AUTO_INCREMENT,
  `account_id` bigint NOT NULL,
  `amount` bigint NOT NULL COMMENT 'can be negative or positive',
  `created_at` datetime NOT NULL DEFAULT (now())
);

CREATE TABLE `transfers` (
  `id` bigint PRIMARY KEY AUTO_INCREMENT,
  `from_account_id` bigint NOT NULL,
  `to_account_id` bigint NOT NULL,
  `amount` bigint NOT NULL COMMENT 'must be positive',
  `created_at` datetime NOT NULL DEFAULT (now())
);

CREATE INDEX `accounts_index_0` ON `accounts` (`owner`);

CREATE INDEX `entries_index_1` ON `entries` (`account_id`);

CREATE INDEX `transfers_index_2` ON `transfers` (`from_account_id`);

CREATE INDEX `transfers_index_3` ON `transfers` (`to_account_id`);

CREATE INDEX `transfers_index_4` ON `transfers` (`from_account_id`, `to_account_id`);

ALTER TABLE `entries` ADD FOREIGN KEY (`account_id`) REFERENCES `accounts` (`id`);

ALTER TABLE `transfers` ADD FOREIGN KEY (`from_account_id`) REFERENCES `accounts` (`id`);

ALTER TABLE `transfers` ADD FOREIGN KEY (`to_account_id`) REFERENCES `accounts` (`id`);
```

### Insert data

```sql
INSERT INTO simple_bank.accounts (id, owner, balance, currency, created_at) VALUES (1, 'one', 100, 'USD', '2020-09-06 15:09:38');
INSERT INTO simple_bank.accounts (id, owner, balance, currency, created_at) VALUES (2, 'two', 100, 'USD', '2020-09-06 15:09:38');
INSERT INTO simple_bank.accounts (id, owner, balance, currency, created_at) VALUES (3, 'three', 100, 'USD', '2020-09-06 15:09:38');
```

### Get transaction isolation level of the current session

```console
mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
```

This level is only applied to this specific MySQL console session. By default, it is `repeatable read` as we can see here.

There's also a global isolation level, which is applied to all sessions when they first started:

```console
mysql> select @@global.transaction_isolation;
+--------------------------------+
| @@global.transaction_isolation |
+--------------------------------+
| REPEATABLE-READ                |
+--------------------------------+
```

By default, it is also `repeatable read`.

### Change isolation level

```console
-- Tx1:
mysql> set session transaction isolation level read uncommitted;
```

Note that this is change will only have effects on all future transactions of this current session, but not on transactions that runs on another session of MySQL console.

[↑ `SET TRANSACTION` statement](https://dev.mysql.com/doc/refman/8.0/en/set-transaction.html).

### Read uncommitted isolation level

Open another terminal window, put it side by side with this one, and start a new MySQL console inside it.

Then let's set the isolation level of this session to read uncommitted as well.

```console
-- Tx2:
mysql> set session transaction isolation level read uncommitted;
```

OK, now both sessions are running at read uncommitted isolation level. We can now start a new transaction.

In MySQL, we can either use `start transaction` statement, or simply use `begin` statement as an alternative.

```console
-- Tx1:
mysql> start transaction;
```

```console
-- Tx2:
mysql> begin;
```

OK, 2 transactions have started. Let's run a simple select from accounts query in transaction 1:

```console
-- Tx1:
mysql> use simple_bank;
mysql> select * from accounts;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |     100 | USD      | 2020-09-06 15:09:38 |
|  2 | two   |     100 | USD      | 2020-09-06 15:09:38 |
|  3 | three |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

At the moment, there are 3 accounts with the same balance of 100 dollars. Then in transaction 2, let's select the first account with id 1:

```console
-- Tx2:
mysql> use simple_bank;
mysql> select * from accounts where id = 1;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

OK, we've got that account with 100 dollars balance. Now let's go back to transaction 1 and run this update statement to subtract 10 dollars from account 1.

```console
-- Tx1:
mysql> update accounts set balance = balance - 10 where id = 1;
```

So, if we select account 1 in transaction 1, we will see that the balance has been changed to 90 dollars:

```console
-- Tx1
mysql> select * from accounts where id = 1;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |      90 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

Transaction 2 can also see the modified value of the balance: 90 dollars, though the transaction 1 is not committed yet:

```console
-- Tx2
mysql> select * from accounts where id = 1;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |      90 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

This is a dirty-read, and it happens because we're using `read uncommitted` isolation level.

Open up a third console, set up isolation level there to `read committed` and make sure you can't see uncommitted changes this time:

```console
-- Tx3
docker exec -it mysql mysql -uroot -p
mysql> use simple_bank;
mysql> set session transaction isolation level read committed;
mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| READ-COMMITTED          |
+-------------------------+
mysql> select * from accounts;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |     100 | USD      | 2020-09-06 15:09:38 |
|  2 | two   |     100 | USD      | 2020-09-06 15:09:38 |
|  3 | three |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

Now, change isolation level of third transaction to `read uncommitted` and try again:

```console
mysql> set session transaction isolation level read uncommitted;
mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| READ-UNCOMMITTED        |
+-------------------------+
mysql> select * from accounts;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |      90 | USD      | 2020-09-06 15:09:38 |
|  2 | two   |     100 | USD      | 2020-09-06 15:09:38 |
|  3 | three |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

You can see uncommitted changes once again.

So `read committed` isolation level prevents dirty read phenomenon.

Commit the first transaction:

-- Tx1:
mysql> commit;

### Read committed isolation level

```console
-- Tx1:
mysql> set session transaction isolation level read committed;
mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| READ-COMMITTED          |
+-------------------------+
```

```console
-- Tx2:
mysql> set session transaction isolation level read committed;
mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| READ-COMMITTED          |
+-------------------------+
```

```console
-- Tx1:
mysql> begin;
mysql> select * from accounts;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |      90 | USD      | 2020-09-06 15:09:38 |
|  2 | two   |     100 | USD      | 2020-09-06 15:09:38 |
|  3 | three |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

```console
-- Tx2:
mysql> begin;
mysql> select * from accounts;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |      90 | USD      | 2020-09-06 15:09:38 |
|  2 | two   |     100 | USD      | 2020-09-06 15:09:38 |
|  3 | three |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

```console
-- Tx1:
mysql> update accounts set balance = balance - 10 where id = 1;
mysql> select * from accounts;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |      80 | USD      | 2020-09-06 15:09:38 |
|  2 | two   |     100 | USD      | 2020-09-06 15:09:38 |
|  3 | three |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

```console
-- Tx2:
mysql> update accounts set balance = balance - 10 where id = 1;
mysql> select * from accounts;
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |      80 | USD      | 2020-09-06 15:09:38 |
|  2 | two   |     100 | USD      | 2020-09-06 15:09:38 |
|  3 | three |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```
