# microk8s

## Installation

Installation [↑ instruction](https://ubuntu.com/kubernetes/install):

```bash
microk8s status --wait-ready
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
```

Print out `.kube/config` file:

```bash
microk8s config
```
