# Helm

- [Helm](#helm)
  - [Installation](#installation)
  - [Commands](#commands)
  - [Create a chart](#create-a-chart)

**Helm** is Kubernetes package manager.

Helm uses a packaging format called _charts_. A **chart** is a collection of files that describe a related set of Kubernetes resources. It is a bundle of information necessary to create an instance of a Kubernetes application.

A **config** is a configuration information that can be merged into a packaged chart to create a releasable object.

A **repository** is the place where charts can be collected and shared.

A **release** is a running in Kubernetes cluster instance of a chart, combined with a specific config. One chart can often be installed many times into the same cluster. And each time it is installed, a new release is created.

## Installation

Homebrew:

```bash
brew install helm
```

## Commands

| Command                                      | Description                                                                      |
| -------------------------------------------- | -------------------------------------------------------------------------------- |
| helm delete RELEASE_NAME                     | Uninstall a release                                                              |
| helm install RELEASE_NAME CHART_NAME         | Install a chart                                                                  |
| helm list                                    | List releases                                                                    |
| helm pull REPOSITORY_NAME/CHART_NAME         | Download a chart from a repository and (optionally) unpack it in local directory |
| helm repo add REPOSITORY_NAME REPOSITORY_URL | Add chart repository                                                             |
| helm repo list                               | List chart repositories                                                          |
| helm repo update                             | Update information of available charts locally from chart repositories           |
| helm status RELEASE_NAME                     | Display status of the release                                                    |

## Create a chart

Generate chart:

```bash
helm create CHART_NAME
```

This will create a new directory called _CHART_NAME_ with the structure shown below:

```text
CHART_NAME
|-- charts
|-- templates
|   |-- tests
|       |-- test-connection.yaml
|   |-- _helpers.tpl
|   |-- deployment.yaml
|   |-- hpa.yaml
|   |-- ingress.yaml
|   |-- NOTES.txt
|   |-- service.yaml
|   |-- serviceaccount.yaml
|-- .helmignore
|-- Chart.yaml
|-- values.yaml
```

Helm runs each file in the _templates_ directory through a Go template rendering engine