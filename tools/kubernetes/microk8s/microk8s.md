# microk8s

## Installation

Installation [↑ instruction](https://ubuntu.com/kubernetes/install):

```bash
sudo snap install microk8s --classic
microk8s status --wait-ready
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
```

Print out `.kube/config` file:

```bash
microk8s config
```

Turn on services you need:

```bash
microk8s enable dns ingress dashboard prometheus
```

## Run dashboard

```bash
kubectl proxy
# http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy
```
