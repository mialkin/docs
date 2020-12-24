# Docker

## Installing

Once Docker is installed, you need to add your user to the docker group (otherwise you’d have to run all docker commands with sudo, which could lead to security issues):

```bash
sudo usermod -aG docker $USER
```

Log out and log back in, so the changes will take effect.

## Images

Command | Description
:-|:-
docker images | Show all top level images, their repository and tags, and their size
docker&nbsp;build&nbsp;-t&nbsp;mialkin/dictionary:3.0&nbsp;. | Build image from a Dockerfile and a current (.) "context". <br/>The repository name will be `mialkin/dictionary` and the tag will be `3.0`
docker pull \<image\> | Pull an image or a repository from a registry. <br/>By default, `docker pull` pulls a *single* image from the registry. A repository can contain multiple images. To pull all images from a repository, provide the `-a` (or `--all-tags`) option when using docker pull. <br/>If no tag is provided, Docker Engine uses the `:latest` tag as a default
docker push mialkin/dictionary:5.0 | Push an image or a repository to a registry
docker rmi \<image1\> \<image2\> | Remove one or more images
docker rmi $(docker images -a -q) | Remove all images
docker save mialkin/dictionary:3.0 \| gzip > dictionary.tar.gz | Save image as gzip archive

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
--name dictionary \
mialkin/dictionary
```

Running options:

Option | Description
:-|:-
 -d | Starts container in detached mode, i.e.  your terminal’s standard input, output, and error (or any combination of the three) will not be attached to running container
--restart unless-stopped | Always restart the container regardless of the exit status, including on daemon startup, except if the container was put into a stopped state before the Docker daemon was stopped
-it|Instructs Docker to allocate a pseudo-TTY connected to the container’s stdin; creating an interactive bash shell in the container
-p 5001:80|Binds TCP port 5001 on the host machine to port 80 of the container
-v&nbsp;/var/app-files:/files| Mounts /var/app-files folder on host machine onto /files folder in container
&#8209;e&nbsp;ADMIN_PASSWORD=yourpass|Sets value of ADMIN_PASSWORD environment variable
--name dictionary|Assigns name to the running container
--rm | Remove container when it exits or when the daemon exits, whichever happens first

## Containers

Command | Description
:-|:-
docker ps | List all currently running containers
docker ps -a | List all containers, including non-running
docker create | The `docker create` command creates a writeable container layer over the specified image and prepares it for running the specified command. The container ID is then printed to STDOUT. This is similar to `docker run -d` except the container is never started. You can then use the `docker start <container_id>` command to start the container at any point.
docker start \<container\> | Start one or more stopped containers
docker attach \<container\> | Attach local standard input, output, and error streams to a running container. <br>To disattach without stoping container press <kbd>Ctrl</kbd> + <kbd>P</kbd>, <kbd>Ctrl</kbd> + <kbd>Q</kbd> (works only for containers started with `-it` option)
docker stop \<container\> | Stop one or more running containers
docker kill $(docker ps -q) | Stop all containers
docker stats [\<container\>] | Display a live stream of container(s) resource usage statistics
docker exec -it \<conainer\> \<command\> | Run a command in a running container
docker attach -f \<container\> | Attach local standard input, output, and error streams to a running container (-f — follow)
docker rm \<container1\> \<container2\> | Remove containers
docker rm $(docker ps -a -q) | Remove all containers
docker port \<container\> | List port mappings or a specific mapping for the container
docker cp /tmp/filename.bak \<container\>:tmp/filename.bak | Copy file from host to container
docker logs -f \<container\> | Fetch the logs of a container; `-f` is for "follow"
docker update --restart=always \<container> | Restart an existing Docker container in restart="always" mode
