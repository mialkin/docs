# docker-compose file reference

- [docker-compose file reference](#docker-compose-file-reference)
  - [build](#build)
  - [depends_on](#depends_on)
  - [image](#image)

## build

Configuration options that are applied at build time.

`build` can be specified either as a string containing a path to the build context:

```yaml
version: "3.9"
services:
    webapp:
        build: ./dir
```

Or, as an object with the path specified under `context` and optionally `Dockerfile` and `args`:

```yaml
version: "3.9"
services:
    webapp:
        build:
            context: ./dir
            dockerfile: Dockerfile-alternate
            args:
                buildno: 1
```

## depends_on

Express dependency between services.
Simple example:

```yaml
version: "3.9"
services:
    web:
        build: .
        depends_on:
            - db
            - redis
    redis:
        image: redis
    db:
        image: postgres
```

Service dependencies cause the following behaviors:

- `docker-compose up` starts services in dependency order. In the following example, `db` and `redis` are started before `web`.
- `docker-compose up SERVICE` automatically includes `SERVICE`’s dependencies. In the example above, `docker-compose up` web also creates and starts `db` and `redis`.
- `docker-compose stop` stops services in dependency order. In the following example, `web` is stopped before `db` and `redis`.

> `depends_on` does not wait for db and redis to be "ready" before starting web - only until they have been started. If you need to wait for a service to be ready, see [Controlling startup](https://docs.docker.com/compose/startup-order/) order for more on this problem and strategies for solving it.

## image

Specify the image to start the container from. Can either be a repository/tag or a partial image ID.

```yaml
image: redis
```

```yaml
image: ubuntu:18.04
```

```yaml
image: tutum/influxdb
```

```yaml
image: example-registry.com:4000/postgresql
```

```yaml
image: a4bc65fd
```

If the image does not exist, Compose attempts to pull it, unless you have also specified `build`, in which case it builds it using the specified options and tags it with the specified tag.
