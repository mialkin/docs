# kubectl

The **kubectl** command line tool lets you control Kubernetes clusters.

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.

## Aliases

Set up aliases:

```sh
echo "alias k='kubectl'" >> ~/.zshrc
```

## Commands

| Command                                                                                          | Description                                                                                                                                |
| ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| kubectl apply -f FILE_NAME.yaml                                                                  | Create objects from file                                                                                                                   |
| cat <<EOF \| kubectl apply -f - <br>MANIFEST_TEXT<br>EOF                                         | Create objects from stdin                                                                                                                  |
| kubectl cluster-info                                                                             | Display addresses of the master and services                                                                                               |
| kubectl config current-context                                                                   | Display the current context                                                                                                                |
| kubectl config delete-context CONTEXT_NAME                                                       | Delete context                                                                                                                             |
| kubectl config get-contexts                                                                      | List all contexts (i.e. clusters)                                                                                                          |
| kubectl config use-context CONTEXT_NAME                                                          | Switch to context                                                                                                                          |
| kubectl config view                                                                              | Show merged kubeconfig settings                                                                                                            |
| kubectl create configmap CONFIGMAP_NAME --from-file=PATH_TO_FILE                                 | Create configmap from file                                                                                                                 |
| kubectl create deployment DEPLOYMENT_NAME --image=IMAGE_NAME                                     | Create deployment directly from image                                                                                                      |
| kubectl create ns NAMESPACE_NAME                                                                 | Create a new namespace                                                                                                                     |
| kubectl delete -f FILENAME.yaml                                                                  | Delete objects described in file                                                                                                           |
| kubectl delete deployment DEPLOYMENT_NAME                                                        | Delete deployment                                                                                                                          |
| kubectl delete namespace NAMESPACE_NAME                                                          | Delete everything from the namespace                                                                                                       |
| kubectl delete service SERVICE_NAME                                                              | Delete service                                                                                                                             |
| kubectl describe configmap CONFIG_MAP_NAME -n NAMESPACE_NAME                                     | Get details of configmap                                                                                                                   |
| kubectl describe deployment DEPLOYMENT_NAME                                                      | Get datails of deployment                                                                                                                  |
| kubectl describe deployments                                                                     | Get details of all deployment                                                                                                              |
| kubectl describe nodes                                                                           | Get details of all nodes                                                                                                                   |
| kubectl exec -it POD_NAME -n NAMESPACE_NAME -- /bin/bash                                         | Run shell inside container inside pod if pod has a single container                                                                        |
| kubectl exec -it POD_NAME -- env                                                                 | Print environment variables of container inside pod if pod has a single container                                                          |
| kubectl exec -it POD_NAME --container CONTAINER_NAME -- /bin/bash                                | Run shell inside container inside pod if pod has several containers                                                                        |
| kubectl get all -n NAMESPACE_NAME                                                                | Show all objects inside namespace                                                                                                          |
| kubectl get all --all-namespaces                                                                 | Display all object from all namespaces                                                                                                     |
| kubectl get certificate --all-namespaces                                                         | Display all certificates                                                                                                                   |
| kubectl get clusterissuer --all-namespaces -o wide                                               | Display cluster issuers                                                                                                                    |
| kubectl get configmaps --all-namespaces                                                          | List configmaps                                                                                                                            |
| kubectl get deployments                                                                          | Display deployments                                                                                                                        |
| kubectl get gateway                                                                              | Show gateways                                                                                                                              |
| kubectl get ep                                                                                   | List endpoints                                                                                                                             |
| kubectl get ingress                                                                              | Show ingresses                                                                                                                             |
| kubectl get ingress -n NAMESPACE_NAME                                                            | Show ingresses in namespace                                                                                                                |
| kubectl get nodes -o wide                                                                        | Get nodes                                                                                                                                  |
| kubectl get ns                                                                                   | Display namespaces in a cluster                                                                                                            |
| kubectl get pods POD_NAME -o=jsonpath='{@}'                                                      | Display pod information in JSON format                                                                                                     |
| kubectl get pods POD_NAME -o jsonpath='{.spec.containers[*].name}'                               | Display container names running in the pod                                                                                                 |
| kubectl get pods -l LABEL_KEY=LABEL_VALUE                                                        | Display pods filtered by label's key and value                                                                                             |
| kubectl get pods -o wide                                                                         | List pods in the namespace                                                                                                                 |
| kubectl get pods --all-namespaces                                                                | List all pods in all namespaces                                                                                                            |
| kubectl get pvc --all-namespaces                                                                 | List persistent volume claims                                                                                                              |
| kubectl get roles --all-namespaces                                                               | List all roles in all namespaces                                                                                                           |
| kubectl get rs                                                                                   | Get replicasets                                                                                                                            |
| kubectl get sc                                                                                   | List storage classes                                                                                                                       |
| kubectl get secret SECRET_NAME -o=yaml                                                           | Show secret in yaml format                                                                                                                 |
| kubectl get secret SECRET_NAME --output="jsonpath={.data.\.dockerconfigjson}" \| base64 --decode | Show secret data in a readable format                                                                                                      |
| kubectl get secrets                                                                              | Display all secrets                                                                                                                        |
| kubectl get serviceaccounts                                                                      | List service accounts                                                                                                                      |
| kubectl get services --watch                                                                     | List all services in the namespace                                                                                                         |
| kubectl get services --all-namespaces                                                            | List services from all namespaces                                                                                                          |
| kubectl get virtualservice                                                                       | Show virtual services                                                                                                                      |
| kubectl label node NODE_NAME LABEL_NAME=LABEL_VALUE                                              | Add label to node                                                                                                                  |
| kubectl label node NODE_NAME LABEL_NAME=-                                                        | Remove label from node                                                                                                                  |
| kubectl logs -f deployment/DEPLOYMENT_NAME                                                       | Show logs for deployment, `-f` means "follow"                                                                                              |
| kubectl logs -f POD_NAME -n NAMESPACE_NAME                                                       | Show logs for pod                                                                                                                          |
| kubectl proxy --port=8080                                                                        | Access service using scheme: ht<span>tp://</span>localhost:8080/api/v1/namespaces/NAMESPACE/services/SERVICE_NAME:SERVICE_PORT_NAME/proxy/ |
| kubectl run -it --rm --restart=Never CONTAINER_NAME --image=IMAGE_NAME sh                        | Run container from image in interactive pod                                                                                                |
| kubectl scale deployment DEPLOYMENT_NAME --replicas=0                                            | Scale the deployment down to 0 replicas                                                                                                    |
| kubectl top nodes                                                                                | Show cluster resource usage                                                                                                                |

## kubectx & kubens

Installation:

```zsh
brew install kubectx
```

Commands:

| Command   | Description                      |
| --------- | -------------------------------- |
| kubectx   | List contexts                    |
| kubectx - | Switch to the previous context   |
| kubens    | List namespaces                  |
| kubens -  | Switch to the previous namespace |

## Multiple clusters

```bash
echo "export KUBECONFIG=~/.kube/config:~/.kube/config-aks" >> ~/.zshrc
source ~/.zshrc
echo $KUBECONFIG
```

## Extract secret to YAML

```bash
kubectl create secret docker-registry SECRET_NAME \
--docker-server="registry.gitlab.com" \
--docker-username="admin" \
--docker-password="abc123" \
#--docker-email="admin@example.com"

kubectl get secret SECRET_NAME -o=yaml
```

## matchLabels and selectors

Only `Job`, `Deployment`, `Replica Set`, and `Daemon Set` support `matchLabels`.
Other object types support `selector`.

## Links

-   [↑ kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
