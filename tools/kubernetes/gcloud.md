# gcloud

The **gcloud** command-line interface is the primary CLI tool to create and manage Google Cloud resources. You can use this tool to perform many common platform tasks either from the command line or in scripts and other automations.

[gcloud command-line tool cheat sheet ↑](https://cloud.google.com/sdk/docs/cheatsheet)

## Container registry

Before you can push or pull images, you must configure Docker to use the gcloud command-line tool to authenticate requests to Container Registry:

Change current project:

```bash
gcloud config set project PROJECT_ID
# gcloud auth login
gcloud auth configure-docker
```

Obtain image and push it:

```bash
docker pull busybox
docker tag busybox gcr.io/PROJECT_ID/busybox
docker push gcr.io/PROJECT_ID/busybox
```
