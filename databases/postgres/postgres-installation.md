# PostgreSQL installation, `pg_dump`, `pg_restore`

## Table of contents

- [PostgreSQL installation, `pg_dump`, `pg_restore`](#postgresql-installation-pg_dump-pg_restore)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
    - [`docker-compose.yml` file](#docker-composeyml-file)
    - [macOS](#macos)
      - [Running](#running)
      - [Connecting first time](#connecting-first-time)
    - [Ubuntu](#ubuntu)
      - [Access locally installed Postgres from Kubernetes pods](#access-locally-installed-postgres-from-kubernetes-pods)
    - [Docker run](#docker-run)
    - [Kubernetes](#kubernetes)
  - [`pg_dump`](#pg_dump)
  - [`pg_restore`](#pg_restore)

## Installation

### `docker-compose.yml` file

```yaml
version: "3.8"

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
```

```bash
docker exec -it postgres psql -U postgres
```

### macOS

```bash
brew install postgresql@16
```

#### Running

```bash
# If you need to have postgresql@16 first in your PATH, run:
echo 'export PATH="/usr/local/opt/postgresql@16/bin:$PATH"' >> ~/.zshrc

# To start postgresql@16 now and restart at login:
brew services start postgresql@16

# Or, if you don't want/need a background service you can just run:
LC_ALL="C" /usr/local/opt/postgresql@16/bin/postgres -D /usr/local/var/postgresql@16
```

#### Connecting first time

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

### Docker run

Starting a postgresql instance. The default `postgres` user and database are created in the entrypoint with [↑ initdb](https://www.postgresql.org/docs/13/app-initdb.html):

```bash
docker run \
--name postgres \
-e POSTGRES_PASSWORD=mysecretpassword \
-d \
-p 5432:5432 postgres

docker exec -it postgres psql -U postgres
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

## `pg_dump`

```bash
pg_dump \
--host=localhost \
--port=MY_PORT \
--dbname=MY_DATABASE \
--username=MY_USERNAME \
> MY_DATABASE.sql
```

```bash
PGPASSWORD="MY_PASSWORD" pg_dump \
-Fc \
--host=localhost \
--port=MY_PORT \
--dbname=MY_DATABASE \
--username=MY_USERNAME \
> MY_DATABASE.dump
```

## `pg_restore`

```bash
pg_restore \
--dbname=MY_DATABASE \
MY_DATABASE.dump
```

```bash
docker exec \
--interactive CONTAINER_NAME pg_restore \
--username=MY_USERNAME \
--verbose \
--dbname=MY_DATABASE < /Users/username/Downloads/MY_DATABASE.dump
```
