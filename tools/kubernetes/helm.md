# Helm

Helm is Kubernetes package manager.

Helm uses a packaging format called _charts_. A chart is a collection of files that describe a related set of Kubernetes resources.

A _repository_ is the place where charts can be collected and shared.

A _release_ is an instance of a chart running in a Kubernetes cluster. One chart can often be installed many times into the same cluster. And each time it is installed, a new release is created.

## Commands

| Command                                      | Description                                                                      |
| -------------------------------------------- | -------------------------------------------------------------------------------- |
| helm delete RELEASE_NAME                     | Uninstall a release                                                              |
| helm install RELEASE_NAME CHART_NAME         | Install a chart                                                                  |
| helm list                                    | List releases                                                                    |
| helm pull REPOSITORY_NAME/CHART_NAME         | Download a chart from a repository and (optionally) unpack it in local directory |
| helm repo list                               | List chart repositories                                                          |
| helm repo add REPOSITORY_NAME REPOSITORY_URL | Add chart repository                                                             |
| helm status RELEASE_NAME                     | Display status of the release                                                    |
