# Docker Compose

**Docker Compose** is a tool for defining and running multi-container Docker applications.

With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

By default, Compose reads two files, a docker-compose.yml and an optional docker-compose.override.yml file. By convention, the docker-compose.yml contains your base configuration.

## Commands

| Command                                       | Description                                                                                                                 |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| docker-compose down --rmi all -v              | Stops containers and removes containers, networks, volumes, and images created by `up`                                      |
| docker-compose kill                           | Forces running containers to stop by sending a SIGKILL signal                                                               |
| docker-compose pull                           | Pulls images associated with a service defined in a docker-compose.yml, but does not start containers based on those images |
| docker-compose start                          | Starts existing containers for a service                                                                                    |
| docker-compose stop                           | Stops running containers without removing them                                                                              |
| docker-compose up                             | Builds, (re)creates, starts, and attaches to containers for a service                                                       |
| docker-compose up -d                          | Run containers in the background in detached mode                                                                           |
| docker-compose up --force-recreate --build -d | Updates existing containers by removing old ones and starting new ones                                                      |

## Options

| Option           | Description                                                                |
| :--------------- | :------------------------------------------------------------------------- |
| -d               | Detached mode: run containers in the background, print new container names |
| --build          | Build images before starting containers                                    |
| --force-recreate | Recreate containers even if their configuration and image haven't changed  |
| --rmi all -v     | The `--rmi all` removes all images; the `-v` removes named volumes         |

## Enable BuildKit

```bash
COMPOSE_DOCKER_CLI_BUILD=0 docker-compose build
```

## Docker Compose file reference

### build

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

### depends_on

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
- `docker-compose up SERVICE` automatically includes `SERVICE`'s dependencies. In the example above, `docker-compose up` web also creates and starts `db` and `redis`.
- `docker-compose stop` stops services in dependency order. In the following example, `web` is stopped before `db` and `redis`.

> `depends_on` does not wait for db and redis to be "ready" before starting web - only until they have been started. If you need to wait for a service to be ready, see [Controlling startup](https://docs.docker.com/compose/startup-order/) order for more on this problem and strategies for solving it.

### image

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
