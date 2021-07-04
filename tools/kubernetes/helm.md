# Helm

- [Helm](#helm)
  - [Install Helm](#install-helm)
  - [Commands](#commands)
  - [Create a chart](#create-a-chart)
  - [Linting](#linting)
  - [Packaging](#packaging)
  - [Deploying](#deploying)
  - [Installing](#installing)

**Helm** is Kubernetes package manager.

Helm uses a packaging format called _charts_. A **chart** is a collection of files that describe a related set of Kubernetes resources. It is a bundle of information necessary to create an instance of a Kubernetes application.

A **config** is a configuration information that can be merged into a packaged chart to create a releasable object.

A **repository** is the place where charts can be collected and shared.

A **release** is a running in Kubernetes cluster instance of a chart, combined with a specific config. One chart can often be installed many times into the same cluster. And each time it is installed, a new release is created.

## Install Helm

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
| helm repo remove REPOSITORY_NAME             | Remove repository                                                                |
| helm repo update                             | Update information of available charts locally from chart repositories           |
| helm show values REPOSITORY_NAME/CHART_NAME  | Display configurable options before installing chart                             |
| helm status RELEASE_NAME                     | Display status of the release's stated, for example during installation          |

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

Helm runs each file in the _templates_ directory through a [↑ Go template](https://golang.org/pkg/text/template/) rendering engine.

You can do a dry-run of a helm install and enable debug to inspect the generated definitions:

```bash
helm install --dry-run --debug --generate-name ./CHART_NAME
```

If a user of your chart wanted to change the default configuration, they could provide overrides directly on the command-line using `--set` parameter:

```bash
helm install --dry-run --generate-name ./CHART_NAME --set service.port=7775
```

## Linting

As you develop your chart, it's a good idea to run it through the linter to ensure you're following best practices and that your templates are well-formed:

```bash
helm lint ./CHART_NAME
```

The _templates/NOTES.txt_ is a templated, plaintext file that gets printed out after the chart is successfully deployed. This is a useful place to briefly describe the next steps for using a chart.

## Packaging

When it's time to package the chart up for distribution, you can run the `helm package` command:

```bash
helm package CHART_NAME/
```

## Deploying

```bash
curl -u<USERNAME>:<PASSWORD> -T <PATH_TO_FILE> "https://mialkin.jfrog.io/artifactory/mialkin-helm/<TARGET_FILE_PATH>"
curl -u "username@gmail.com:YOUR_PASSWOR" -T PACKAGE_NAME.tgz "https://mialkin.jfrog.io/artifactory/mialkin-helm/CHART_NAME"
```

## Installing

Installing chart from local files:

```bash
helm install RELEASE_NAME ./mychart --set service.type=NodePort
```

Installing chart from local package:

```bash
helm package RELEASE_NAME PACKAGE_NAME.tgz
```

Installing from jFrog repository:

```bash
helm repo update
helm install mialkin-helm/[chartName]
```

Helm installs resources in the following order:

- Namespace
- NetworkPolicy
- ResourceQuota
- LimitRange
- PodSecurityPolicy
- PodDisruptionBudget
- ServiceAccount
- Secret
- SecretList
- ConfigMap
- StorageClass
- PersistentVolume
- PersistentVolumeClaim
- CustomResourceDefinition
- ClusterRole
- ClusterRoleList
- ClusterRoleBinding
- ClusterRoleBindingList
- Role
- RoleList
- RoleBinding
- RoleBindingList
- Service
- DaemonSet
- Pod
- ReplicationController
- ReplicaSet
- Deployment
- HorizontalPodAutoscaler
- StatefulSet
- Job
- CronJob
- Ingress
- APIService
