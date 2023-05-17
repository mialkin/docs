# PostgreSQL

- [PostgreSQL](#postgresql)
  - [Installation](#installation)
    - [Local installation](#local-installation)
      - [Access from Kubernetes pods](#access-from-kubernetes-pods)
    - [Docker run](#docker-run)
    - [docker-compose](#docker-compose)
    - [Kubernetes](#kubernetes)
  - [SQL](#sql)
    - [Create database](#create-database)
  - [psql](#psql)
    - [Commands](#commands)
    - [Shortcuts](#shortcuts)
  - [JSON](#json)

## Installation

### Local installation

```bash
sudo apt install postgresql postgresql-contrib
```

[↑ How To Install PostgreSQL on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-22-04-quickstart)

Check status:

```bash
sudo systemctl status postgresql.service
sudo systemctl restart postgresql.service
```

Create new user:

```bash
sudo -i -u postgres
createuser --interactive
exit
```

Enter something like:

```text
Enter name of role to add: dictionary
Shall the new role be a superuser? (y/n) y
```

Another assumption that the Postgres authentication system makes by default is that for any role used to log in, that role will have a database with the same name which it can access.

This means that if the user you created in the last section is called `dictionary`, that role will attempt to connect to a database which is also called `dictionary` by default. You can create the appropriate database with the createdb command.

```bash
createdb dictionary
```

Set new password for user:

```bash
psql
ALTER USER dictionary PASSWORD 'dictionary';
# If successful, Postgres will output a confirmation of ALTER ROLE.
\q
```

#### Access from Kubernetes pods

Enable the DNS and host-access addons:

```bash
microk8s.enable dns host-access
```

Host-access will bind the host to an IP within your cluster, the default being `10.0.1.1`. Check if Postgres listens on the desired address by running:

```bash
sudo ss -ntlp | grep postgres
```

From your pods deployed within your Microk8s cluster you should be able to reach IP addresses of your node:

```bash
apt update
apt install iputils-ping
ping 10.0.1.1
```

Configure PostgreSQL to accept all incoming connections by opening client authentication configuration file:

```bash
sudo vim /etc/postgresql/14/main/pg_hba.conf
```

> Modification of pg_hba.conf file may not be needed. Try first without modifying it

Add this line:

```text
host    all             all             0.0.0.0/0               md5
```

Install Postgres client inside your pod:

```bash
apt install -y postgresql-client
psql --version 
```

Try to connect to database:

```bash
psql postgresql://dictionary@10.0.1.1:5432/dictionary?sslmode=disable
```

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

## JSON

[↑ Querying JSON columns](https://www.npgsql.org/efcore/mapping/json.html#querying-json-columns)
