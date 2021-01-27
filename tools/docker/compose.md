# docker-compose

**docker-compose** is a tool for defining and running multi-container Docker applications.

With docker-compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

## Commands

| Command                                       | Description                                                                                                                                                 |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| docker-compose up -d                          | Run containers in the background, print new container names.                                                                                                |
| docker-compose down --rmi all -v              | Stops containers and removes containers, networks, volumes, and images created by `up`. The `--rmi all` removes all images; the `-v` removes named volumes. |
| docker-compose pull                           | Pulls an image associated with a service defined in a docker-compose.yml, but does not start containers based on those images.                              |
| docker-compose up --force-recreate --build -d | Updates existing container by removing old one and starting a new one.                                                                                      |
| docker image prune -f                         | Removes unused images.                                                                                                                                      |
