# Docker Compose

**Compose** is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

## Commands

Command                          | Description
---------------------------------|----------------------
docker-compose up -d             | Run containers in the background, print new container names. |
docker-compose down --rmi all -v | Stops containers and removes containers, networks, volumes, and images created by `up`. The `--rmi all` removes all images; the `-v` removes named volumes.
docker-compose up --force-recreate --build -d | Updates existing container by removing old one and starting a new one.
docker image prune -f | Removes unused images.
## Links

* [↑ Install Docker Compose](https://docs.docker.com/compose/install/)
* [↑ Overview of docker-compose CLI](https://docs.docker.com/compose/reference/overview/)
* [↑ Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
