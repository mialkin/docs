# k3s

Print out `.kube/config` file:

```bash
sudo cat /etc/rancher/k3s/k3s.yaml
```

Check out Kubernetes version:

```bash
kubectl version --short
```

## Dashboard

Install dashboard: https://rancher.com/docs/k3s/latest/en/installation/kube-dashboard/

```bash
kubectl proxy
kubectl -n kubernetes-dashboard describe secret admin-user-token | grep '^token'
```

## Applications

| Application name | YAML file                                |
| ---------------- | ---------------------------------------- |
| whoami           | [whoami.yaml](whoami.yaml)               |
| Jenkins          | [jenkins.yaml](jenkins.yaml)             |
| Grafana          | [grafana.yaml](grafana.yaml)             |
