# Docker

## Installing

On Linux, once Docker is installed, you need to add your user to the docker group (otherwise you’d have to run all docker commands with `sudo`, which could lead to security issues):

```bash
sudo usermod -aG docker $USER
```

Log out and log back in, so the changes will take effect.

## Commands

| Command                                                | Description                                                                                                                                                                                                                                                         |
| :----------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| docker attach -f CONTAINER                             | Attach local standard input, output, and error streams to a running container, `-f` — follow. <br>To disattach without stoping container press <kbd>Ctrl</kbd> + <kbd>P</kbd>, <kbd>Ctrl</kbd> + <kbd>Q</kbd> (works only for containers started with `-it` option) |
| docker build -t TAG .                                  | Build image                                                                                                                                                                                                                                                         |
| docker container prune                                 | Remove all stopped containers                                                                                                                                                                                                                                       |
| docker cp /tmp/filename.bak CONTAINER:tmp/filename.bak | Copy file from host to container                                                                                                                                                                                                                                    |
| docker create IMAGE                                    | Create a new container from image without running it. <br>To start container run `docker start CONTAINER`.                                                                                                                                                          |
| docker exec -it CONTAINER COMMAND                      | Run a command in a running container                                                                                                                                                                                                                                |
| docker image prune -f                                  | Remove all dangling images. <br/>A dangling image is one that is not tagged and is not referenced by any container                                                                                                                                                  |
| docker image prune -a                                  | Remove all images without at least one container associated to them                                                                                                                                                                                                 |
| docker images                                          | Display images                                                                                                                                                                                                                                                      |
| docker kill $(docker ps -q)                            | Stop all containers                                                                                                                                                                                                                                                 |
| docker login                                           | Log in to a Docker registry                                                                                                                                                                                                                                         |
| docker login REGISTRY_NAME -u USERNAME -p ACCESS_TOKEN | Log in to a registry using username and access token                                                                                                                                                                                                                |
| docker logout                                          | Log out from a Docker registry, i.e. removes credentials from ~/.docker/config.json file                                                                                                                                                                            |
| docker logout private.example.com                      | Log out from private repository                                                                                                                                                                                                                                     |
| docker logs -f CONTAINER                               | Fetch the logs of a container; `-f` is for "follow"                                                                                                                                                                                                                 |
| docker port CONTAINER                                  | List port mappings or a specific mapping for the container                                                                                                                                                                                                          |
| docker ps -a                                           | List all containers, including non-running                                                                                                                                                                                                                          |
| docker pull HOSTNAME/PROJECT-ID/IMAGE:TAG              | Pull image from registry                                                                                                                                                                                                                                            |
| docker push HOSTNAME/PROJECT-ID/IMAGE:TAG              | Push image to a registry                                                                                                                                                                                                                                            |
| docker rm CONTAINER                                    | Remove one or more containers                                                                                                                                                                                                                                       |
| docker rm $(docker ps -a -q)                           | Remove all containers                                                                                                                                                                                                                                               |
| docker rmi IMAGE                                       | Remove one or more images                                                                                                                                                                                                                                           |
| docker rmi $(docker images -a -q)                      | Remove all images                                                                                                                                                                                                                                                   |
| docker save IMAGE \| gzip > ARCHIVE.tar.gz             | Save image as gzip archive                                                                                                                                                                                                                                          |
| docker start CONTAINER                                 | Start one or more stopped containers                                                                                                                                                                                                                                |
| docker stats                                           | Display a live stream of containers resource usage statistics                                                                                                                                                                                                       |
| docker stats CONTAINER                                 | Display a live stream of a single container resource usage statistics                                                                                                                                                                                               |
| docker stop CONTAINER                                  | Stop one or more running containers                                                                                                                                                                                                                                 |
| docker update --restart=always CONTAINER               | Update configuration of one or more containers setting restart policy to apply when a container exits                                                                                                                                                               |
| docker volume ls                                       | List all the volumes known to Docker.                                                                                                                                                                                                                               |

## Options

| Option                   | Description                                                                                                                                                                        |
| :----------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -d                       | Starts container in detached mode, i.e. your terminal’s standard input, output, and error (or any combination of the three) will not be attached to running container              |
| -e KEY=VALUE             | Sets environment variable                                                                                                                                                          |
| -it                      | Instructs Docker to allocate a pseudo-TTY connected to the container’s stdin; creating an interactive bash shell in the container                                                  |
| -p 5001:80               | Binds TCP port 5001 on the host machine to port 80 of the container                                                                                                                |
| -v /var/app-files:/files | Mounts /var/app-files folder on host machine onto /files folder in container                                                                                                       |
| --name CONTAINER_NAME    | Assigns name to the running container                                                                                                                                              |
| --restart unless-stopped | Always restart the container regardless of the exit status, including on daemon startup, except if the container was put into a stopped state before the Docker daemon was stopped |
| --rm                     | Remove container when it exits or when the daemon exits, whichever happens first                                                                                                   |

Example of `run` command:

```sh
docker run \
-d \
--restart unless-stopped \
-it \
-p 5000:80 \
-v /var/app-files:/files \
-e ADMIN_PASSWORD=yourpass \
--name CONTAINER_NAME \
IMAGE_NAME
```
