# PostgreSQL. `psql`. `pg_dump`. `dropdb`. `createdb`. `pg_restore`

## Table of contents

- [PostgreSQL. `psql`. `pg_dump`. `dropdb`. `createdb`. `pg_restore`](#postgresql-psql-pg_dump-dropdb-createdb-pg_restore)
  - [Table of contents](#table-of-contents)
  - [PostgreSQL](#postgresql)
    - [Docker](#docker)
    - [macOS](#macos)
    - [Ubuntu](#ubuntu)
      - [Access locally installed Postgres from Kubernetes pods](#access-locally-installed-postgres-from-kubernetes-pods)
    - [Kubernetes](#kubernetes)
  - [Client applications](#client-applications)
    - [`psql`](#psql)
      - [Meta-commands](#meta-commands)
      - [Create role](#create-role)
    - [`pg_dump`](#pg_dump)
    - [`dropdb`](#dropdb)
    - [`createdb`](#createdb)
    - [`pg_restore`](#pg_restore)

## PostgreSQL

### Docker

`docker-compose.yml` file:

```yaml
services:
  database:
    image: postgres:16.2
    container_name: postgres
    restart: unless-stopped
    ports:
      - "8420:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    volumes:
      - ./volumes/postgres:/var/lib/postgresql/data
```

```bash
docker exec -it postgres psql -U postgres
```

### macOS

```bash
brew install postgresql@16
```

Running:

```bash
# If you need to have postgresql@16 first in your PATH, run:
echo 'export PATH="/usr/local/opt/postgresql@16/bin:$PATH"' >> ~/.zshrc

# To start postgresql@16 now and restart at login:
brew services start postgresql@16

# Or, if you don't want/need a background service you can just run:
LC_ALL="C" /usr/local/opt/postgresql@16/bin/postgres -D /usr/local/var/postgresql@16
```

Connecting first time:

```bash
psql postgres
```

Create `postgres` user as superuser:

```bash
createuser -s postgres
```

Restart PostgreSQL.

### Ubuntu

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

#### Access locally installed Postgres from Kubernetes pods

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

### Kubernetes

Install Postgres with [↑ Bitmani helm chart](https://bitnami.com/stack/postgresql/helm):

```bash
helm install postgres bitnami/postgresql
```

To get information needed to connect to database run:

```bash
helm status postgres
```

## Client applications

[↑ PostgreSQL client applications](https://postgrespro.ru/docs/postgresql/16/reference-client?lang=en).

### `psql`

[↑ `psql`](https://postgrespro.ru/docs/postgresql/16/app-psql?lang=en) is a terminal-based front-end to PostgreSQL.

#### Meta-commands

Anything you enter in `psql` that begins with an unquoted backslash is a `psql` _meta-command_ that is processed by `psql` itself.

Meta-commands are often called _slash commands_ or _backslash commands_.

| Meta-command       | Description                                           |
| ------------------ | ----------------------------------------------------- |
| `\\?`              | Show available commands                               |
| `\c`               | Show current database name                            |
| `\c DATABASE_NAME` | Switch connection to the database                     |
| `\d TABLE_NAME`    | Describe table                                        |
| `\dn`              | List schemas                                          |
| `\dt` or `\dt+`    | List all tables in the current database               |
| `\du`              | List roles                                            |
| `\l` or `l+`       | List all databases                                    |
| `\q`               | Quit psql                                             |
| `\! clear`         | As an alternative use `Ctrl + L` or `\! cls` commands |

#### Create role

[↑ `CREATE ROLE`](https://postgrespro.ru/docs/postgresql/16/sql-createrole?lang=en).

```sql
SELECT * FROM pg_user;
SELECT * FROM pg_roles;
```

### `pg_dump`

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

### `dropdb`

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

### `createdb`

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

### `pg_restore`

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
docker exec --interactive CONTAINER_NAME \
pg_restore \
--username=MY_USERNAME \
--dbname=MY_DATABASE \
--verbose \
< MY_DATABASE.dump
```

[↑ `pg_restore`](https://postgrespro.ru/docs/postgresql/16/app-pgrestore?lang=en).
