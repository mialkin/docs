# Docker

## Installing

Once Docker is installed, you need to add your user to the docker group (otherwise you’d have to run all docker commands with sudo, which could lead to security issues):

```bash
sudo usermod -aG docker $USER
```

Log out and log back in, so the changes will take effect.

## Commands

| Command                                              | Description                                                                                                                                                                                                                                                                                                               |
| :--------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| docker build -t TAG_NAME .                           | Build image                                                                                                                                                                                                                                                                                                               |
| docker create                                        | Create a new container. This command creates a writeable container layer over the specified image and prepares it for running the specified command. This is similar to `docker run -d` except the container is never started. You can then use the `docker start CONTAINER` command to start the container at any point. |
| docker images                                        | Display images                                                                                                                                                                                                                                                                                                            |
| docker login                                         | Log in to a Docker registry                                                                                                                                                                                                                                                                                               |
| docker ps -a                                         | List all containers, including non-running                                                                                                                                                                                                                                                                                |
| docker pull IMAGE_NAME                               | Pull image                                                                                                                                                                                                                                                                                                                |
| docker push IMAGE_NAME                               | Push an image to a registry                                                                                                                                                                                                                                                                                               |
| docker rmi IMAGE_NAME                                | Remove one or more images (separated by space)                                                                                                                                                                                                                                                                            |
| docker rmi $(docker images -a -q)                    | Remove all images                                                                                                                                                                                                                                                                                                         |
| docker save IMAGE_NAME \| gzip > ARCHIVE_NAME.tar.gz | Save image as gzip archive                                                                                                                                                                                                                                                                                                |
| docker start CONTAINER                               | Start one or more stopped containers (separated by space)                                                                                                                                                                                                                                                                 |

## Running image

Example of `run` command:

```sh
sudo docker run \
-d \
--restart unless-stopped \
-it \
-p 5000:80 \
-v /var/app-files:/files \
-e ADMIN_PASSWORD=yourpass \
--name CONTAINER_NAME \
IMAGE_NAME
```

Running options:

| Option                                | Description                                                                                                                                                                        |
| :------------------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -d                                    | Starts container in detached mode, i.e. your terminal’s standard input, output, and error (or any combination of the three) will not be attached to running container              |
| --restart unless-stopped              | Always restart the container regardless of the exit status, including on daemon startup, except if the container was put into a stopped state before the Docker daemon was stopped |
| -it                                   | Instructs Docker to allocate a pseudo-TTY connected to the container’s stdin; creating an interactive bash shell in the container                                                  |
| -p 5001:80                            | Binds TCP port 5001 on the host machine to port 80 of the container                                                                                                                |
| -v&nbsp;/var/app-files:/files         | Mounts /var/app-files folder on host machine onto /files folder in container                                                                                                       |
| &#8209;e&nbsp;ADMIN_PASSWORD=yourpass | Sets value of ADMIN_PASSWORD environment variable                                                                                                                                  |
| --name CONTAINER_NAME                 | Assigns name to the running container                                                                                                                                              |
| --rm                                  | Remove container when it exits or when the daemon exits, whichever happens first                                                                                                   |

## Containers

| Command                                                | Description                                                                                                                                                                                                                                          |
| :----------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| docker attach CONTAINER                                | Attach local standard input, output, and error streams to a running container. <br>To disattach without stoping container press <kbd>Ctrl</kbd> + <kbd>P</kbd>, <kbd>Ctrl</kbd> + <kbd>Q</kbd> (works only for containers started with `-it` option) |
| docker stop CONTAINER                                  | Stop one or more running containers                                                                                                                                                                                                                  |
| docker kill $(docker ps -q)                            | Stop all containers                                                                                                                                                                                                                                  |
| docker stats [CONTAINER]                               | Display a live stream of container(s) resource usage statistics                                                                                                                                                                                      |
| docker exec -it \<conainer\> \<command\>               | Run a command in a running container                                                                                                                                                                                                                 |
| docker attach -f CONTAINER                             | Attach local standard input, output, and error streams to a running container (-f — follow)                                                                                                                                                          |
| docker rm \<container1\> \<container2\>                | Remove containers                                                                                                                                                                                                                                    |
| docker rm $(docker ps -a -q)                           | Remove all containers                                                                                                                                                                                                                                |
| docker port CONTAINER                                  | List port mappings or a specific mapping for the container                                                                                                                                                                                           |
| docker cp /tmp/filename.bak CONTAINER:tmp/filename.bak | Copy file from host to container                                                                                                                                                                                                                     |
| docker logs -f CONTAINER                               | Fetch the logs of a container; `-f` is for "follow"                                                                                                                                                                                                  |
| docker update --restart=always \<container>            | Restart an existing Docker container in restart="always" mode                                                                                                                                                                                        |
