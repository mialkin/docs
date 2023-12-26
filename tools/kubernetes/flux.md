# Flux

[↑ Flux](https://fluxcd.io/flux) is a tool for keeping Kubernetes clusters in sync with sources of configuration, like Git repositories, and automating updates to configuration when there is new code to deploy.

## Table of contents

- [Flux](#flux)
  - [Table of contents](#table-of-contents)
  - [Install](#install)
  - [Uninstall](#uninstall)
  - [Commands](#commands)
  - [Core concepts](#core-concepts)
    - [Source](#source)
    - [Kustomization](#kustomization)
  - [GitLab](#gitlab)

## Install

```bash
brew install fluxcd/tap/flux
# brew upgrade fluxcd/tap/flux

# Check if your Kubernetes cluster has everything needed to run Flux
flux check --pre
```

## Uninstall

```bash
brew uninstall fluxcd/tap/flux
```

Note that the uninstall command will not remove any Kubernetes objects or Helm releases that were reconciled on the cluster by Flux. It is safe to uninstall Flux and rerun the bootstrap, any existing workloads will not be affected.

## Commands

| Command                                                | Description                                   |
| ------------------------------------------------------ | --------------------------------------------- |
| flux get image policy                                  |                                               |
| flux get image repository NAME                         |                                               |
| flux get images update                                 |                                               |
| flux get kustomizations                                | Get `Kustomization` statuses                  |
| flux get kustomizations --watch                        | Start watching kustomizations                 |
| flux get sources all                                   | Get all source statuses                       |
| flux get sources git                                   | List `GitRepository` sources and their status |
| flux suspend kustomization KUSTOMIZATION_NAME          | Suspend reconciliation of `Kustomization`     |
| flux delete kustomization KUSTOMIZATION_NAME           | Delete a `Kustomization` resource             |
| flux delete source git SOURCE_NAME                     | Delete a `GitRepository` source               |
| flux reconcile kustomization flux-system --with-source | Pull and apply the changes                    |
| flux stats                                             | Stats of Flux reconciles                      |
| flux version                                           |                                               |

## Core concepts

### Source

A **source** defines the origin of a repository containing the desired state of the system and the requirements to obtain it. The origin of the source is checked for changes on a defined interval, if there is a newer version available that matches the criteria, a new artifact is produced.

All sources are specified as [↑ custom resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) in a Kubernetes cluster, examples of sources are [`↑ GitRepository`](https://fluxcd.io/flux/components/source/gitrepositories), `OCIRepository`, `HelmRepository` and `Bucket` resources.

### Kustomization

A `Kustomization` custom resource represents a local set of Kubernetes resources that Flux is supposed to reconcile in the cluster. The reconciliation runs every five minutes by default, but this can be changed with `.spec.interval`.

## GitLab
