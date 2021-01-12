# kubectl

The **kubectl** command line tool lets you control Kubernetes clusters.

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.

## Aliases

Set up aliases:

```sh
echo "alias k='kubectl'" >> ~/.zshrc
```

| Command                                                                   | Description                                                                       |
| ------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| kubectl apply -f deployment.yaml                                          | Create deployment                                                                 |
| kubectl cluster-info                                                      | Display addresses of the master and services                                      |
| kubectl config current-context                                            | Display the current context                                                       |
| kubectl config delete-context CONTEXT_NAME                                | Delete context                                                                    |
| kubectl config get-contexts                                               | List all contexts (i.e. clusters)                                                 |
| kubectl config use-context CONTEXT_NAME                                   | Switch to context                                                                 |
| kubectl config view                                                       | Show merged kubeconfig settings                                                   |
| kubectl create ns NAMESPACE_NAME                                          | Create a new namespace                                                            |
| kubectl delete -f gateway.yaml                                            | Delete gateway                                                                    |
| kubectl delete -f virtual-service.yaml                                    | Delete virtual service                                                            |
| kubectl delete -f deployment-service.yaml                                 | Delete deployment and service                                                     |
| kubectl delete deployment DEPLOYMENT_NAME                                 | Delete deployment                                                                 |
| kubectl delete service SERVICE_NAME                                       | Delete service                                                                    |
| kubectl describe deployment DEPLOYMENT_NAME                               | Get datails of deployment                                                         |
| kubectl describe deployments                                              | Get details of all deployment                                                     |
| kubectl exec -it POD_NAME -- /bin/bash                                    | Run shell inside container inside pod if pod has a single container               |
| kubectl exec -it POD_NAME -- env                                          | Print environment variables of container inside pod if pod has a single container |
| kubectl exec -it POD_NAME --container CONTAINER_NAME -- /bin/bash         | Run shell inside container inside pod if pod has several containers               |
| kubectl get deployments                                                   | Display deployments                                                               |
| kubectl get gateway                                                       | Show gateways                                                                     |
| kubectl get namespaces                                                    | List the current namespaces in a cluster                                          |
| kubectl get pods POD_NAME -o=jsonpath='{@}'                               | Display pod information in JSON format                                            |
| kubectl get pods POD_NAME -o jsonpath='{.spec.containers[*].name}'        | Display container names running in the pod                                        |
| kubectl get pods --output wide                                            | List pods in the namespace                                                        |
| kubectl get pods --all-namespaces                                         | List all pods in all namespaces                                                   |
| kubectl get rs                                                            | Get replicasets                                                                   |
| kubectl get services --watch                                              | List all services in the namespace                                                |
| kubectl get virtualservice                                                | Show virtual services                                                             |
| kubectl run -it --rm --restart=Never CONTAINER_NAME --image=IMAGE_NAME sh | Run container from image in interactive pod                                       |
| kubectl scale deployment DEPLOYMENT_NAME --replicas=3                     | Scale the deployment to 3 replicas                                                |

## Links

- [↑ kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
