# kubectl

The **kubectl** command line tool lets you control Kubernetes clusters.

## Table of contents

- [kubectl](#kubectl)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Commands](#commands)
  - [k9s](#k9s)
  - [Multiple clusters](#multiple-clusters)
  - [Create Docker registry secret](#create-docker-registry-secret)
  - [Create secret from literals](#create-secret-from-literals)
  - [Decode base64 string](#decode-base64-string)
  - [Extract secret to YAML](#extract-secret-to-yaml)
  - [Debug or test pod inside Kubernetes cluster](#debug-or-test-pod-inside-kubernetes-cluster)
  - [DNS lookup](#dns-lookup)

## Installation

```bash
brew install kubectl
```

## Commands

| Command                                                                                          | Description                                                                                                                                     |
| ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| kubectl apply -f FILE_NAME.yaml                                                                  | Create objects from file                                                                                                                        |
| cat <<EOF \| kubectl apply -f - <br>MANIFEST_TEXT<br>EOF                                         | Create objects from stdin                                                                                                                       |
| kubectl cluster-info                                                                             | Display addresses of the master and services                                                                                                    |
| kubectl config current-context                                                                   | Display the current context                                                                                                                     |
| kubectl config delete-context CONTEXT_NAME                                                       | Delete context                                                                                                                                  |
| kubectl config get-contexts                                                                      | List all contexts (i.e. clusters)                                                                                                               |
| kubectl config use-context CONTEXT_NAME                                                          | Switch to context                                                                                                                               |
| kubectl config view                                                                              | Show merged kubeconfig settings                                                                                                                 |
| kubectl cp /path/to/your_folder name-of-your-pod:/path/to/destination_folder                     | Copy files from local computer to pod                                                                                                           |
| kubectl cp dict-79b7cdc496-l6sq7:/app/SQLite-files/dictionary.db dictionary.db -n mialkin        | Copy files from pod to current local folder                                                                                                     |
| kubectl create configmap CONFIGMAP_NAME --from-file=PATH_TO_FILE                                 | Create configmap from file                                                                                                                      |
| kubectl create deployment DEPLOYMENT_NAME --image=IMAGE_NAME                                     | Create deployment directly from image                                                                                                           |
| kubectl create ns NAMESPACE_NAME                                                                 | Create a new namespace                                                                                                                          |
| kubectl delete -f FILENAME.yaml                                                                  | Delete objects described in file                                                                                                                |
| kubectl delete deployment DEPLOYMENT_NAME                                                        | Delete deployment                                                                                                                               |
| kubectl delete namespace NAMESPACE_NAME                                                          | Delete everything from the namespace                                                                                                            |
| kubectl delete pod POD_NAME                                                                      | Delete pod by its name                                                                                                                          |
| kubectl delete service SERVICE_NAME                                                              | Delete service                                                                                                                                  |
| kubectl describe challenges                                                                      | Shows why an ACME Order is not being finished                                                                                                   |
| kubectl describe configmap CONFIG_MAP_NAME -n NAMESPACE_NAME                                     | Get details of configmap                                                                                                                        |
| kubectl describe deployment DEPLOYMENT_NAME                                                      | Get datails of deployment                                                                                                                       |
| kubectl describe deployments                                                                     | Get details of all deployment                                                                                                                   |
| kubectl describe nodes                                                                           | Get details of all nodes, inluding used resources (CPU & memory requests) and current performance                                               |
| kubectl drain NODE_NAME                                                                          | Draining didn't work in GKE as was expected                                                                                                     |
| kubectl exec -it POD_NAME -n NAMESPACE_NAME -- /bin/bash                                         | Run shell inside container inside pod if pod has a single container                                                                             |
| kubectl exec -it POD_NAME -- env                                                                 | Print environment variables of container inside pod if pod has a single container                                                               |
| kubectl exec -it POD_NAME --container CONTAINER_NAME -- /bin/bash                                | Run shell inside container inside pod if pod has several containers                                                                             |
| kubectl get all -n NAMESPACE_NAME                                                                | Show all objects inside namespace                                                                                                               |
| kubectl get all --all-namespaces                                                                 | Display all object from all namespaces                                                                                                          |
| kubectl get certificate --all-namespaces                                                         | Display all certificates                                                                                                                        |
| kubectl get clusterissuer --all-namespaces -o wide                                               | Display cluster issuers                                                                                                                         |
| kubectl get configmaps --all-namespaces                                                          | List configmaps                                                                                                                                 |
| kubectl get deployments                                                                          | Display deployments                                                                                                                             |
| kubectl get gateway                                                                              | Show gateways                                                                                                                                   |
| kubectl get ep                                                                                   | List endpoints                                                                                                                                  |
| kubectl get ingress                                                                              | Show ingresses                                                                                                                                  |
| kubectl get ingress -n NAMESPACE_NAME                                                            | Show ingresses in namespace                                                                                                                     |
| kubectl get nodes -o wide                                                                        | Get nodes                                                                                                                                       |
| kubectl get ns                                                                                   | Display namespaces in a cluster                                                                                                                 |
| kubectl get pods POD_NAME -o=jsonpath='{@}'                                                      | Display pod information in JSON format                                                                                                          |
| kubectl get pods POD_NAME -o jsonpath='{.spec.containers[*].name}'                               | Display container names running in the pod                                                                                                      |
| kubectl get pods -l LABEL_KEY=LABEL_VALUE                                                        | Display pods filtered by label's key and value                                                                                                  |
| kubectl get pods -o wide                                                                         | List pods in the namespace                                                                                                                      |
| kubectl get pods --all-namespaces                                                                | List all pods in all namespaces                                                                                                                 |
| kubectl get pvc --all-namespaces                                                                 | List persistent volume claims                                                                                                                   |
| kubectl get roles --all-namespaces                                                               | List all roles in all namespaces                                                                                                                |
| kubectl get rs                                                                                   | Get replica sets                                                                                                                                |
| kubectl get sc                                                                                   | List storage classes                                                                                                                            |
| kubectl get secret SECRET_NAME -o=yaml                                                           | Show secret in yaml format                                                                                                                      |
| kubectl get secret SECRET_NAME --output="jsonpath={.data.\.dockerconfigjson}" \| base64 --decode | Show secret data in a readable format                                                                                                           |
| kubectl get secrets                                                                              | Display all secrets                                                                                                                             |
| kubectl get serviceaccounts                                                                      | List service accounts                                                                                                                           |
| kubectl get services --watch                                                                     | List all services in the namespace                                                                                                              |
| kubectl get services --all-namespaces                                                            | List services from all namespaces                                                                                                               |
| kubectl get virtualservice                                                                       | Show virtual services                                                                                                                           |
| kubectl label node NODE_NAME LABEL_NAME=LABEL_VALUE                                              | Add label to node                                                                                                                               |
| kubectl label node NODE_NAME LABEL_NAME=-                                                        | Remove label from node                                                                                                                          |
| kubectl logs -f deployment/DEPLOYMENT_NAME                                                       | Show logs for deployment, `-f` means "follow"                                                                                                   |
| kubectl logs -f POD_NAME -n NAMESPACE_NAME                                                       | Show logs for pod                                                                                                                               |
| kubectl plugin list                                                                              | Find any kubectl plugins in your PATH                                                                                                           |
| kubectl port-forward --namespace default svc/postgres-postgresql 5432:5432                       | Access and interact with internal Kubernetes cluster processes from your localhost. Kill processes: `pkill -f "port-forward"`                   |
| kubectl port-forward redis-master-765d459796-258hz 6379:6379                                     | Port-forward pod                                                                                                                                |
| kubectl proxy --port=8080                                                                        | Access service using scheme: ht<span>tp://</span>localhost:8080/api/v1/namespaces/NAMESPACE<br/>/services/SERVICE_NAME:SERVICE_PORT_NAME/proxy/ |
| kubectl run POD_SLASH_CONTAINER_NAME --image=IMAGE_NAME                                          | Create container from a particular image and run it in a pod.                                                                                   |
| kubectl run CONTAINER_NAME --image=IMAGE_NAME                                                    | Run container from image                                                                                                                        |
| kubectl run -it --rm --restart=Never CONTAINER_NAME --image=IMAGE_NAME sh                        | Run container from image in interactive pod                                                                                                     |
| kubectl scale deployment DEPLOYMENT_NAME --replicas=0                                            | Scale the deployment down to 0 replicas                                                                                                         |
| kubectl top nodes                                                                                | Show cluster resource usage                                                                                                                     |

[↑ Kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## k9s

Installation:

```bash
brew install k9s
```

Update:

```bash
brew upgrade k9s
```

Project on GitHub: https://github.com/derailed/k9s.

## Multiple clusters

```bash
echo "export KUBECONFIG=~/.kube/config:~/.kube/config-aks" >> ~/.zshrc
source ~/.zshrc
echo $KUBECONFIG
```

## Create Docker registry secret

```bash
kubectl create secret docker-registry SECRET_NAME \
--namespace="YOUR_NAMESPACE" \
--docker-server="registry.gitlab.com" \
--docker-username="admin" \
--docker-password="abc123"
#--docker-email="admin@example.com"
```

## Create secret from literals

```bash
kubectl create secret generic SECRET_NAME \
--namespace="YOUR_NAMESPACE" \
--from-literal="username=admin" \
--from-literal="password=123" \
--from-literal="connectionstring='host=localhost;user=admin;password=secret'"
```

## Decode base64 string

```bash
echo QWxhZGRpbjpvcGVuIHNlc2FtZQ== | base64 --decode
```

## Extract secret to YAML

```bash
kubectl get secret SECRET_NAME -o=yaml
```

## Debug or test pod inside Kubernetes cluster

```bash
kubectl run \
-it \
--tty \
--rm debug \
--image=alpine \
-n mialkin \
--restart=Never -- sh

apk update && apk add curl # Install curl
apk add busybox-extras # Install telnet

curl dictionary-api.dictionary.svc.cluster.local/index.html
telnet postgres.tools.svc.cluster.local 5432
```

[↑ Create the various debug or test pod inside kubernetes cluster](https://medium.com/@shambhand2020/create-the-various-debug-or-test-pod-inside-kubernetes-cluster-e4862c767b96).

## DNS lookup

Inside pod run:

```bash
apt update
apt install dnsutils
nslookup YOUR_SERVICE_NAME
```

This will output something like:

```text
Server:         10.152.183.10
Address:        10.152.183.10#53

Name:   YOUR_SERVICE_NAME.YOUR_NAMESPACE.svc.cluster.local
Address: 10.152.183.131
```
