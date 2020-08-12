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

## Containers

Command | Description
:-|:-
|
|
|
|
|
