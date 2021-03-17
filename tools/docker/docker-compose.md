# docker-compose

**docker-compose** is a tool for defining and running multi-container Docker applications.

With docker-compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

## Commands

| Command                                       | Description                                                                                                                   |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| docker-compose down --rmi all -v              | Stops containers and removes containers, networks, volumes, and images created by `up`                                        |
| docker-compose pull                           | Pulls images associated with a service defined in a docker-compose.yml, but does not start containers based on those images |
| docker-compose up                             | Builds, (re)creates, starts, and attaches to containers for a service                                                         |
| docker-compose up -d                          | Run containers in the background in detached mode                                                                             |
| docker-compose up --force-recreate --build -d | Updates existing containers by removing old ones and starting new ones                                                        |

## Options

| Option           | Description                                                                |
| :--------------- | :------------------------------------------------------------------------- |
| -d               | Detached mode: run containers in the background, print new container names |
| --build          | Build images before starting containers                                    |
| --force-recreate | Recreate containers even if their configuration and image haven't changed  |
| --rmi all -v     | The `--rmi all` removes all images; the `-v` removes named volumes         |
