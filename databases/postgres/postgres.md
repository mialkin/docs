# PostgreSQL

- [PostgreSQL](#postgresql)
  - [SQL](#sql)
    - [Create database](#create-database)
  - [psql](#psql)
    - [Commands](#commands)
    - [Shortcuts](#shortcuts)
  - [JSON](#json)

## SQL

### Create database

When you get a connection to PostgreSQL it is always to a particular database. To access a different database, you must get a new connection.

Using `\c` in psql closes the old connection and acquires a new one, using the specified database and/or credentials. You get a whole new back-end process and everything.

```sql
create database DATABASE_NAME;
select current_database(); -- Display current database name
drop database if exists DATABASE_NAME; -- if exists is optional
```

## psql

**psql** is a terminal-based front-end to PostgreSQL. It enables you to type in queries interactively, issue them to PostgreSQL, and see the query results:

```bash
docker exec -it postgres psql -U postgres
```

### Commands

| Command          | Description                             |
| ---------------- | --------------------------------------- |
| \\?              | Show available commands                 |
| \c               | Show current database name              |
| \c DATABASE_NAME | Switch connection to the database       |
| \d TABLE_NAME    | Describe table                          |
| \dn              | List schemas                            |
| \dt              | List all tables in the current database |
| \l               | List all databases                      |
| \q               | Quit psql                               |

### Shortcuts

| Shortcut | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| Ctrl + L | Clear screen. As an alternative use `\! clear` or `\! cls` commands |

## JSON

[↑ Querying JSON columns](https://www.npgsql.org/efcore/mapping/json.html#querying-json-columns)
