# Docker

## Images

Command | Description
:-|:-
docker images | Show all top level images, their repository and tags, and their size
docker&nbsp;build&nbsp;-&nbsp;t&nbsp;mialkin/dictionary:3.0&nbsp;. | Build image from a Dockerfile and a current (.) "context". <br/>The repository name will be `mialkin/dictionary` and the tag will be `3.0`
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
-p 5001:80 \
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
docker stop \<container id\> | Stop running container
docker kill $(docker ps -q) | Stop all containers
docker stats | 