# RabbitMQ

## Kubernetes installation

Install `rabbitmq` plugin using `krew` kubectl plugin manager:

```bash
kubectl krew install rabbitmq
kubectl rabbitmq version
kubectl rabbitmq help
```

Install [cluster operator ↑](https://www.rabbitmq.com/kubernetes/operator/operator-overview.html):

```bash
kubectl rabbitmq install-cluster-operator
```

## Commands

| Command                                                             | Description                                   |
| ------------------------------------------------------------------- | --------------------------------------------- |
| kubectl rabbitmq -n YOUR_NAMESPACE create CLUSTER_NAME --replicas 1 | Create cluster                                |
| kubectl rabbitmq delete CLUSTER_NAME                                | Delete cluster                                |
| kubectl rabbitmq get CLUSTER_NAME                                   | Display all resources associated with cluster |
| kubectl rabbitmq list                                               | List all clusters                             |
| kubectl rabbitmq secrets CLUSTER_NAME                               | Display default user secrets                  |
