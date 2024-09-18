# Docker Compose

[↑ **Docker Compose**](https://docs.docker.com/compose/intro/history/) is a tool for running multi-container applications on Docker defined using the Compose file format.

## Commands

| Command                                       | Description                                                                                                                 |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| docker compose down --rmi all -v              | Stops containers and removes containers, networks, volumes, and images created by `up`                                      |
| docker compose kill                           | Forces running containers to stop by sending a SIGKILL signal                                                               |
| docker compose pull                           | Pulls images associated with a service defined in a docker-compose.yml, but does not start containers based on those images |
| docker compose start                          | Starts existing containers for a service                                                                                    |
| docker compose stop                           | Stops running containers without removing them                                                                              |
| docker compose up                             | Builds, (re)creates, starts, and attaches to containers for a service                                                       |
| docker compose up --detach                    | Run containers in the background in detached mode                                                                           |
| docker compose up --force-recreate --build --detach | Updates existing containers by removing old ones and starting new ones                                                      |

## Enable BuildKit

```bash
COMPOSE_DOCKER_CLI_BUILD=0 docker-compose build
```

## Docker Compose file reference

### build

Configuration options that are applied at build time.

`build` can be specified either as a string containing a path to the build context:

```yaml
services:
  webapp:
    build: ./dir
```

Or, as an object with the path specified under `context` and optionally `Dockerfile` and `args`:

```yaml
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

- `docker compose up` starts services in dependency order. In the following example, `db` and `redis` are started before `web`.
- `docker compose up SERVICE` automatically includes `SERVICE`'s dependencies. In the example above, `docker compose up` web also creates and starts `db` and `redis`.
- `docker compose stop` stops services in dependency order. In the following example, `web` is stopped before `db` and `redis`.

`depends_on` does not wait for db and redis to be "ready" before starting web - only until they have been started. If you need to wait for a service to be ready, see [Control startup and shutdown order in Compose](https://docs.docker.com/compose/startup-order/).
