# PostregSQL

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

Start executing commands:

```bash
docker exec -it CONTAINER_NAME psql -U postgres
```

## Shortcuts

| Shortcut | Description                                                         |
| -------- | ------------------------------------------------------------------- |
| CTRL + L | Clear screen. As an alternative use `\! clear` or `\! cls` commands |

## Commands

| Command       | Description                             |
| ------------- | --------------------------------------- |
| \?            | Show available commands                 |
| \c DB_NAME    | Switch connection to the database       |
| \d TABLE_NAME | Describe table                          |
| \dt           | List all tables in the current database |
| \l or \list   | List all databases                      |
| \q            | Quit psql                               |

## Links

[↑ Quick reference](https://github.com/docker-library/docs/blob/master/postgres/README.md)

[↑ PostgreSQL 13.0 Documentation](https://www.postgresql.org/docs/13/index.html)

[↑ initdb](https://www.postgresql.org/docs/current/app-initdb.html)

[↑ psql — PostgreSQL interactive terminal](https://www.postgresql.org/docs/13/app-psql.html)
