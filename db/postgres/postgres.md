# PostregSQL

Starting a postgresql instance. The default `postgres` user and database are created in the entrypoint with [↑ initdb](https://www.postgresql.org/docs/13/app-initdb.html):

```bash
docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```

Start executing commands:

```bash
docker exec -it some-postgres psql -U postgres
```

## Shortcuts

Shortcut | Description  
---------|--------------
CTRL + L | Clear screen. As an alternative use `\! clear` or `\! cls` commands

## Commands

Command     | Description
------------|-------------
\l or \list | List all databases

## Links

[↑ Quick reference](https://github.com/docker-library/docs/blob/master/postgres/README.md)

[↑ PostgreSQL 13.0 Documentation](https://www.postgresql.org/docs/13/index.html)

[↑ initdb](https://www.postgresql.org/docs/current/app-initdb.html)

[↑ psql — PostgreSQL interactive terminal](https://www.postgresql.org/docs/13/app-psql.html)
