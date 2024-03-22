# psql, SQL

## Table of contents

- [psql, SQL](#psql-sql)
  - [Table of contents](#table-of-contents)
  - [psql](#psql)
    - [Meta-commands](#meta-commands)
  - [SQL](#sql)
    - [Create database](#create-database)
    - [Shortcuts](#shortcuts)
  - [JSON](#json)

## psql

[↑ psql](https://postgrespro.ru/docs/postgresql/16/app-psql?lang=en) is a terminal-based front-end to PostgreSQL. It enables you to type in queries interactively, issue them to PostgreSQL, and see the query results.

### Meta-commands

Anything you enter in psql that begins with an unquoted backslash is a psql **meta-command** that is processed by psql itself. These commands make psql more useful for administration or scripting. Meta-commands are often called **slash** or **backslash commands**.

| Meta-command     | Description                             |
| ---------------- | --------------------------------------- |
| \\?              | Show available commands                 |
| \c               | Show current database name              |
| \c DATABASE_NAME | Switch connection to the database       |
| \d TABLE_NAME    | Describe table                          |
| \dn              | List schemas                            |
| \dt              | List all tables in the current database |
| \l               | List all databases                      |
| \q               | Quit psql                               |

## SQL

### Create database

When you get a connection to PostgreSQL it is always to a particular database. To access a different database, you must get a new connection.

Using `\c` in psql closes the old connection and acquires a new one, using the specified database and/or credentials. You get a whole new back-end process and everything.

```sql
create database DATABASE_NAME;
select current_database(); -- Display current database name
drop database if exists DATABASE_NAME; -- if exists is optional
```

### Shortcuts

| Shortcut | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| Ctrl + L | Clear screen. As an alternative use `\! clear` or `\! cls` commands |

## JSON

[↑ Querying JSON columns](https://www.npgsql.org/efcore/mapping/json.html#querying-json-columns)
