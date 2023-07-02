# Isolation levels and read phenomena

- [Isolation levels and read phenomena](#isolation-levels-and-read-phenomena)
  - [Read phenomena](#read-phenomena)
    - [Dirty reads](#dirty-reads)
    - [Non-repeatable reads](#non-repeatable-reads)
    - [Phantom reads](#phantom-reads)
  - [Isolation levels](#isolation-levels)
    - [Read uncommitted](#read-uncommitted)
    - [Read committed](#read-committed)
    - [Repeatable read](#repeatable-read)
    - [Serializable](#serializable)
  - [DataGrip](#datagrip)
  - [Links](#links)
  - [Isolation levels in MySQL](#isolation-levels-in-mysql)
    - [Run MySQL](#run-mysql)
    - [Get transaction isolation level of the current session](#get-transaction-isolation-level-of-the-current-session)
    - [Change isolation level](#change-isolation-level)

An **isolation level** represents a particular locking strategy employed in the database system to avoid _read phenomena_.

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

## Isolation levels

The American National Standards Institute (ANSI) defines four isolation levels:

- Read uncommitted
- Read committed
- Repeatable read
- Serializable

See 1995 article: [A Critique of ANSI SQL Isolation Levels](files/tr-95-51.pdf).

The plus sign `+` reads as "phenomenon can happen":

| Isolation level  | Phantom reads | Non-repeatable reads | Dirty reads |
| ---------------- | ------------- | -------------------- | ----------- |
| Read uncommitted | +             | +                    | +           |
| Read committed   | +             | +                    | -           |
| Repeatable read  | +             | -                    | -           |
| Serializable     | -             | -                    | -           |

Although higher isolation levels provide better data consistency, this consistency can be costly in terms of the parallel access provided to users. As isolation level increases, so does the chance that the locking strategy used will create problems with parallel access of data.

### Read uncommitted

Locks are obtained on modifications to the database and held until end of transaction (EOT). Reading from the database does not involve any locking.

### Read committed

Locks are acquired for reading and modifying the database. Locks are released after reading but locks on modified objects are held until EOT.

### Repeatable read

Locks are obtained for reading and modifying the database. Locks on all modified objects are held until EOT. Locks obtained for reading data are held until EOT. Locks on non-modified access structures (such as indexes and hashing structures) are released after reading.

### Serializable

All data read or modified is locked until EOT. All access structures that are modified are locked until EOT. Access structures used by the query are locked until EOT.

## DataGrip

Selection of an isolation level in DataGrip:

<img src="files/datagrip.png" width="350" />

## Links

[↑ Deeply understand Isolation levels and Read phenomena in MySQL & PostgreSQL](https://dev.to/techschoolguru/understand-isolation-levels-read-phenomena-in-mysql-postgres-c2e)

## Isolation levels in MySQL

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

### Get transaction isolation level of the current session

```console
mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
1 row in set (0.01 sec)
```

This level is only applied to this specific MySQL console session. By default, it is `repeatable read` as we can see here.

There’s also a global isolation level, which is applied to all sessions when they first started:

```console
mysql> select @@global.transaction_isolation;
+--------------------------------+
| @@global.transaction_isolation |
+--------------------------------+
| REPEATABLE-READ                |
+--------------------------------+
1 row in set (0.00 sec)
```

By default, it is also `repeatable read`.

### Change isolation level

```console
mysql> set session transaction isolation level read uncommitted;
Query OK, 0 rows affected (0.00 sec)
```

Note that this is change will only have effects on all future transactions of this current session, but not on transactions that runs on another session of MySQL console.
