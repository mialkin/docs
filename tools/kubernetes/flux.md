# Flux

[↑ Flux](https://fluxcd.io/flux) is a tool for keeping Kubernetes clusters in sync with sources of configuration, like Git repositories, and automating updates to configuration when there is new code to deploy.

## Table of contents

- [Flux](#flux)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Install or upgrade Flux](#install-or-upgrade-flux)
  - [Uninstall Flux](#uninstall-flux)
  - [Commands](#commands)
  - [GitLab](#gitlab)
  - [GitHub](#github)
  - [Repository structure](#repository-structure)
  - [Setting up application](#setting-up-application)
  - [Automate image updates to Git](#automate-image-updates-to-git)
  - [Core concepts](#core-concepts)
    - [Source](#source)
    - [Kustomization](#kustomization)
    - [Reconciliation](#reconciliation)

## Installation

[↑ Installation](https://fluxcd.io/flux/installation/)

Install Flux CLI:

```bash
brew install fluxcd/tap/flux
# brew upgrade fluxcd/tap/flux
```

Check if your Kubernetes cluster has everything needed to run Flux:

```bash
flux check --pre
```

## Install or upgrade Flux

For testing purposes you can install Flux onto Kubernetes cluster without storing its manifests in a Git repository:

```bash
flux install
```

## Uninstall Flux

[↑ Uninstall](https://fluxcd.io/flux/installation/#uninstall) Flux from Kubernetes cluster:

```bash
flux uninstall
```

Note that the `uninstall` command will not remove any Kubernetes objects or Helm releases that were reconciled on the cluster by Flux. It is safe to uninstall Flux and rerun the bootstrap, any existing workloads will not be affected.

## Commands

| Command                                         | Description                                   |
| ----------------------------------------------- | --------------------------------------------- |
| `flux get sources all`                          | Get all source statuses                       |
| `flux get sources git`                          | List `GitRepository` sources and their status |
| `flux get kustomizations`                       | Get `Kustomization` statuses                  |
| `flux suspend kustomization KUSTOMIZATION_NAME` | Suspend reconciliation of `Kustomization`     |
| `flux delete kustomization KUSTOMIZATION_NAME`  | Delete a `Kustomization` resource             |
| `flux delete source git SOURCE_NAME`            | Delete a `GitRepository` source               |
| `flux stats`                                    | Stats of Flux reconciles                      |
| `flux version`                                  |                                               |

## GitLab

The `bootstrap gitlab` command creates a GitLab repository if one doesn't exist and commits the Flux components manifests to specified branch. Then it configures the target cluster to synchronize with that repository by setting up an SSH deploy key or by using token-based authentication.

Generate a [↑ personal access token](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html) that grants complete read/write access to the GitLab API.

Export your GitLab personal access token as an environment variable:

```bash
export GITLAB_TOKEN=<your-token>
```

Run the bootstrap for a repository on your personal GitLab account:

```bash
flux bootstrap gitlab \
  --owner=my-gitlab-username \
  --repository=my-repository \
  --branch=main \
  --path=clusters/my-cluster \
  --token-auth \
  --personal
```

## GitHub

Export your username and your [↑ access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token):

```bash
export GITHUB_TOKEN=<your-token>
export GITHUB_USER=<your-username>
```

Install Flux onto your cluster:

```bash
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=fleet-infra \
  --branch=main \
  --path=./clusters/my-cluster \
  --personal
```

## Repository structure

[↑ Repo per app](https://fluxcd.io/flux/guides/repository-structure/#repo-per-app).

## Setting up application

Clone Flux repository. From its root folder run:

```bash
mkdir clusters/zotac/dictionary-api

flux create source git dictionary-api \
  --url=https://github.com/mialkin/dictionary-api \
  --branch=main \
  --interval=1m0s \
  --export > ./clusters/zotac/dictionary-api/dictionary-api-source.yaml
```

```bash
git add .
git commit -am "Add GitRepository source"
git push
```

Start watching kustomization:

```bash
flux get kustomizations --watch
```

Again, from the root folder of Flux repository, but in a separate terminal, run:

```bash
flux create kustomization dictionary-api \
  --target-namespace=dictionary \
  --source=dictionary-api \
  --path="./deploy/manifests" \
  --prune=true \
  --interval=5m0s \
  --export > ./clusters/zotac/dictionary-api/dictionary-api-kustomization.yaml
```

## Automate image updates to Git

[↑ Automate image updates to Git](https://fluxcd.io/flux/guides/image-update).

## Core concepts

### Source

A **source** defines the origin of a repository containing the desired state of the system and the requirements to obtain it.

The origin of the source is checked for changes on a defined interval, if there is a newer version available that matches the criteria, a new artifact is produced.

All sources are specified as [↑ custom resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) in a Kubernetes cluster, examples of sources are [`↑ GitRepository`](https://fluxcd.io/flux/components/source/gitrepositories), `OCIRepository`, `HelmRepository` and `Bucket` resources.

### Kustomization

A `Kustomization` custom resource represents a local set of Kubernetes resources that Flux is supposed to reconcile in the cluster. The reconciliation runs every five minutes by default, but this can be changed with `.spec.interval`.

### Reconciliation

Reconciliation refers to ensuring that a given state (e.g. application running in the cluster, infrastructure) matches a desired state declaratively defined somewhere (e.g. a Git repository).

There are various examples of these in Flux:

- `HelmRelease` reconciliation: ensures the state of the Helm release matches what is defined in the resource, performs a release if this is not the case (including revision changes of a HelmChart resource).
- [↑ `Bucket`](https://fluxcd.io/flux/components/source/buckets/) reconciliation: downloads and archives the contents of the declared bucket on a given interval and stores this as an artifact, records the observed revision of the artifact and the artifact itself in the status of resource.
- `Kustomization` reconciliation: ensures the state of the application deployed on a cluster matches the resources defined in a Git repository or S3 bucket.
