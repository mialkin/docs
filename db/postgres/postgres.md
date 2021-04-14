# PostregSQL

## Kubernetes

```bash
helm install postgres bitnami/postgresql
```

PostgreSQL can be accessed via port 5432 on the following DNS name from within your cluster: 

```text
postgres-postgresql.default.svc.cluster.local
```

To get the password for "postgres" run:

```bash
export POSTGRES_PASSWORD=$(kubectl get secret --namespace default postgres-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode)
```

To connect to your database from outside the cluster execute the following commands:

```bash
kubectl port-forward --namespace default svc/postgres-postgresql 5432:5432 PGPASSWORD="YOUR_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432
```

To connect to your database run the following command:

```bash
kubectl run postgres-postgresql-client --rm --tty -i --restart='Never' --namespace default --image docker.io/bitnami/postgresql:11.11.0-debian-10-r31 --env="PGPASSWORD=YOUR_PASSWORD" --command -- psql --host postgres-postgresql -U postgres -d postgres -p 5432
```

## Docker

Starting a postgresql instance. The default `postgres` user and database are created in the entrypoint with [↑ initdb](https://www.postgresql.org/docs/13/app-initdb.html):

```bash
docker run --name CONTAINER_NAME -e POSTGRES_PASSWORD=mysecretpassword -d -p 5432:5432 postgres
```

Using docker-compose:

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

## psql

**psql** is a terminal-based front-end to PostgreSQL. It enables you to type in queries interactively, issue them to PostgreSQL, and see the query results. Alternatively, input can be from a file. In addition, it provides a number of meta-commands and various shell-like features to facilitate writing scripts and automating a wide variety of tasks.

Start executing commands:

```bash
docker exec -it CONTAINER_NAME psql -U postgres
```

## Shortcuts

| Shortcut | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| CTRL + L | Clear screen. As an alternative use `\! clear` or `\! cls` commands |

## Commands

| Command          | Description                             |
| ---------------- | --------------------------------------- |
| \\?              | Show available commands                 |
| \c DATABASE_NAME | Switch connection to the database       |
| \d TABLE_NAME    | Describe table                          |
| \dn              | List schemas                            |
| \dt              | List all tables in the current database |
| \l or \list      | List all databases                      |
| \q               | Quit psql                               |

## SQL syntax

| SQL                                      | Description   |
| ---------------------------------------- | ------------- |
| DROP DATABASE [IF EXISTS] DATABASE_NAME; | Drop database |

## Links

[↑ Quick reference](https://github.com/docker-library/docs/blob/master/postgres/README.md)

[↑ PostgreSQL 13.0 Documentation](https://www.postgresql.org/docs/13/index.html)

[↑ initdb](https://www.postgresql.org/docs/current/app-initdb.html)

[↑ psql — PostgreSQL interactive terminal](https://www.postgresql.org/docs/13/app-psql.html)
