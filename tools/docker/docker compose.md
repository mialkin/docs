# Docker Compose

**Docker Compose** is a tool for defining and running multi-container Docker applications.

With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

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

## Useful links

- [↑ Environment variables](https://docs.docker.com/compose/environment-variables/)
- [↑ Variable substitution](https://docs.docker.com/compose/compose-file/compose-file-v3/#variable-substitution)
- [↑ Multiple Compose files](https://docs.docker.com/compose/extends/#multiple-compose-files)
- [↑ Volume configuration reference](https://docs.docker.com/compose/compose-file/compose-file-v3/#volume-configuration-reference)
