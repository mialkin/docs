# Docker

## Installing

Once Docker is installed, you need to add your user to the docker group (otherwise you’d have to run all docker commands with sudo, which could lead to security issues):

```bash
sudo usermod -aG docker $USER
```

Log out and log back in, so the changes will take effect.

## Commands

| Command                                                | Description                                                                                                                                                                                                                                                         |
| :----------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| docker attach -f CONTAINER                             | Attach local standard input, output, and error streams to a running container, `-f` — follow. <br>To disattach without stoping container press <kbd>Ctrl</kbd> + <kbd>P</kbd>, <kbd>Ctrl</kbd> + <kbd>Q</kbd> (works only for containers started with `-it` option) |
| docker build -t TAG .                                  | Build image                                                                                                                                                                                                                                                         |
| docker cp /tmp/filename.bak CONTAINER:tmp/filename.bak | Copy file from host to container                                                                                                                                                                                                                                    |
| docker create IMAGE                                    | Create a new container from image without running it. <br>To start container run `docker start CONTAINER`.                                                                                                                                                          |
| docker exec -it CONTAINER COMMAND                      | Run a command in a running container                                                                                                                                                                                                                                |
| docker images                                          | Display images                                                                                                                                                                                                                                                      |
| docker kill $(docker ps -q)                            | Stop all containers                                                                                                                                                                                                                                                 |
| docker login                                           | Log in to a Docker registry                                                                                                                                                                                                                                         |
| docker logs -f CONTAINER                               | Fetch the logs of a container; `-f` is for "follow"                                                                                                                                                                                                                 |
| docker port CONTAINER                                  | List port mappings or a specific mapping for the container                                                                                                                                                                                                          |
| docker ps -a                                           | List all containers, including non-running                                                                                                                                                                                                                          |
| docker pull IMAGE                                      | Pull image                                                                                                                                                                                                                                                          |
| docker push IMAGE                                      | Push an image to a registry                                                                                                                                                                                                                                         |
| docker rm CONTAINER                                    | Remove one or more containers                                                                                                                                                                                                                                       |
| docker rm $(docker ps -a -q)                           | Remove all containers                                                                                                                                                                                                                                               |
| docker rmi IMAGE                                       | Remove one or more images                                                                                                                                                                                                                                           |
| docker rmi $(docker images -a -q)                      | Remove all images                                                                                                                                                                                                                                                   |
| docker save IMAGE \| gzip > ARCHIVE.tar.gz             | Save image as gzip archive                                                                                                                                                                                                                                          |
| docker start CONTAINER                                 | Start one or more stopped containers                                                                                                                                                                                                                                |
| docker stats CONTAINER                                 | Display a live stream of container(s) resource usage statistics                                                                                                                                                                                                     |
| docker stop CONTAINER                                  | Stop one or more running containers                                                                                                                                                                                                                                 |
| docker update --restart=always CONTAINER               | Update configuration of one or more containers setting restart policy to apply when a container exits                                                                                                                                                               |

## Running image

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
IMAGE
```

### Options explanation

| Option                                | Description                                                                                                                                                                        |
| :------------------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -d                                    | Starts container in detached mode, i.e. your terminal’s standard input, output, and error (or any combination of the three) will not be attached to running container              |
| --restart unless-stopped              | Always restart the container regardless of the exit status, including on daemon startup, except if the container was put into a stopped state before the Docker daemon was stopped |
| -it                                   | Instructs Docker to allocate a pseudo-TTY connected to the container’s stdin; creating an interactive bash shell in the container                                                  |
| -p 5001:80                            | Binds TCP port 5001 on the host machine to port 80 of the container                                                                                                                |
| -v /var/app-files:/files              | Mounts /var/app-files folder on host machine onto /files folder in container                                                                                                       |
| &#8209;e&nbsp;ADMIN_PASSWORD=yourpass | Sets value of ADMIN_PASSWORD environment variable                                                                                                                                  |
| --name CONTAINER_NAME                 | Assigns name to the running container                                                                                                                                              |
| --rm                                  | Remove container when it exits or when the daemon exits, whichever happens first                                                                                                   |
