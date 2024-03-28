# PostgreSQL client applications

[↑ PostgreSQL client applications](https://postgrespro.ru/docs/postgresql/16/reference-client?lang=en).

## Table of contents

- [PostgreSQL client applications](#postgresql-client-applications)
  - [Table of contents](#table-of-contents)
  - [`psql`](#psql)
    - [Meta-commands](#meta-commands)
  - [`pg_dump`](#pg_dump)
  - [`dropdb`](#dropdb)
  - [`createdb`](#createdb)
  - [`pg_restore`](#pg_restore)

## `psql`

[↑ `psql`](https://postgrespro.ru/docs/postgresql/16/app-psql?lang=en) is a terminal-based front-end to PostgreSQL.

### Meta-commands

Anything you enter in `psql` that begins with an unquoted backslash is a `psql` _meta-command_ that is processed by `psql` itself.

Meta-commands are often called _slash commands_ or _backslash commands_.

| Meta-command       | Description                                           |
| ------------------ | ----------------------------------------------------- |
| `\\?`              | Show available commands                               |
| `\c`               | Show current database name                            |
| `\c DATABASE_NAME` | Switch connection to the database                     |
| `\d TABLE_NAME`    | Describe table                                        |
| `\dn`              | List schemas                                          |
| `\dt`              | List all tables in the current database               |
| `\l`               | List all databases                                    |
| `\q`               | Quit psql                                             |
| `\! clear`         | As an alternative use `Ctrl + L` or `\! cls` commands |

## `pg_dump`

Dump a database called `MY_DATABASE` into an SQL-script file:

```bash
PGPASSWORD="MY_PASSWORD" \
pg_dump \
--host=localhost \
--port=1234 \
--username=MY_USERNAME \
--verbose \
MY_DATABASE \
> MY_DATABASE.sql
```

Dump a database into a custom-format archive file:

```bash
PGPASSWORD="MY_PASSWORD" \
pg_dump \
-Fc \
--host=localhost \
--port=1234 \
--username=MY_USERNAME \
--verbose \
MY_DATABASE \
> MY_DATABASE.dump
```

[↑ `pg_dump`](https://postgrespro.ru/docs/postgresql/16/app-pgdump?lang=en).

## `dropdb`

Regular:

```bash
PGPASSWORD="MY_PASSWORD" \
dropdb \
--host=localhost \
--port=1234 \
--username=MY_USERNAME \
MY_DATABASE
```

Inside Docker:

```bash
docker exec --interactive --tty CONTAINER_NAME \
dropdb \
--username=MY_USERNAME \
MY_DATABASE
```

[↑ `dropdb`](https://postgrespro.ru/docs/postgresql/16/app-dropdb?lang=en).

## `createdb`

Regular:

```bash
PGPASSWORD="MY_PASSWORD" \
createdb \
--host=localhost \
--port=1234 \
--username=MY_USERNAME \
MY_DATABASE
```

Inside Docker:

```bash
docker exec --interactive --tty CONTAINER_NAME \
createdb \
--username=MY_USERNAME \
MY_DATABASE
```

[↑ `createdb`](https://postgrespro.ru/docs/postgresql/16/app-createdb?lang=en).

## `pg_restore`

Restore dump to database, beforehand deleted with [`dropdb`](#dropdb) and recreated with [`createdb`](#createdb).

Regular:

```bash
PGPASSWORD="MY_PASSWORD" \
pg_restore \
--host=localhost \
--port=1234 \
--username=MY_USERNAME \
--dbname=MY_DATABASE \
--verbose \
< MY_DATABASE.dump
```

Inside Docker:

```bash
docker exec --interactive --tty CONTAINER_NAME \
pg_restore \
--username=MY_USERNAME \
--dbname=MY_DATABASE \
--verbose \
< MY_DATABASE.dump
```

[↑ `pg_restore`](https://postgrespro.ru/docs/postgresql/16/app-pgrestore?lang=en).
