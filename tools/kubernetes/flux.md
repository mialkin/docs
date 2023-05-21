# Flux

[↑ Flux](https://fluxcd.io/flux) is a tool for keeping Kubernetes clusters in sync with sources of configuration (like Git repositories), and automating updates to configuration when there is new code to deploy.

[↑ Get Started with Flux](https://fluxcd.io/flux/get-started)

Install Flux:

```bash
brew install fluxcd/tap/flux
```

Uninstall Flux and delete CRDs:

```bash
flux uninstall
```

Check if your Kubernetes cluster has everything needed to run Flux:

```bash
flux check --pre
```

## Core concepts

A **source** defines the origin of a repository containing the desired state of the system and the requirements to obtain it.

The origin of the source is checked for changes on a defined interval, if there is a newer version available that matches the criteria, a new artifact is produced.

All sources are specified as [↑ custom resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) in a Kubernetes cluster, examples of sources are [`↑ GitRepository`](https://fluxcd.io/flux/components/source/gitrepositories), `OCIRepository`, `HelmRepository` and `Bucket` resources.

## Commands

| Command                                        | Description                                   |
| ---------------------------------------------- | --------------------------------------------- |
| `flux get sources all`                         | Get all source statuses                       |
| `flux get sources git`                         | List `GitRepository` sources and their status |
| `flux get kustomizations`                      | Get `Kustomization` statuses                  |
| `flux delete kustomization KUSTOMIZATION_NAME` | Delete a `Kustomization` resource             |
| `flux delete source git SOURCE_NAME`           | Delete a `GitRepository` source               |

## Something else

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
