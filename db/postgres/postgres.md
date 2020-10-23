# PostregSQL

Starting a postgresql instance with the default `postgres` user and database are created in the entrypoint with `initdb`:

```bash
docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```

[↑ Quick reference](https://github.com/docker-library/docs/blob/master/postgres/README.md)

[↑ PostgreSQL 13.0 Documentation](https://www.postgresql.org/docs/13/index.html)
