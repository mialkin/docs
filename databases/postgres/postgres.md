# PostgreSQL

- [PostgreSQL](#postgresql)
  - [Installation](#installation)
    - [Docker run](#docker-run)
    - [docker-compose](#docker-compose)
    - [Kubernetes](#kubernetes)
  - [SQL](#sql)
    - [Create database](#create-database)
  - [psql](#psql)
    - [Commands](#commands)
    - [Shortcuts](#shortcuts)

## Installation

### Docker run

Starting a postgresql instance. The default `postgres` user and database are created in the entrypoint with [↑ initdb](https://www.postgresql.org/docs/13/app-initdb.html):

```bash
docker run --name postgres -e POSTGRES_PASSWORD=mysecretpassword -d -p 5432:5432 postgres
docker exec -it postgres psql -U postgres
```

### docker-compose

```yaml
version: "3.8"
services:
    db:
        image: postgres
        container_name: CONTAINER_NAME
        restart: unless-stopped
        ports:
            - 5432:5432
        environment:
            POSTGRES_PASSWORD: mysecretpassword

    adminer:
        image: adminer
        restart: unless-stopped
        ports:
            - 8080:8080
```

### Kubernetes

Install Postgres with [↑ Bitmani helm chart](https://bitnami.com/stack/postgresql/helm):

```bash
helm install postgres bitnami/postgresql
```

To get information needed to connect to database run:

```bash
helm status postgres
```

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
