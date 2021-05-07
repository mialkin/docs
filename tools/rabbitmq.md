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

## Running cluster

```bash
kubectl rabbitmq -n YOUR_NAMESPACE create CLUSTER_NAME --replicas 1
kubectl rabbitmq -n YOUR_NAMESPACE secrets CLUSTER_NAME
kubectl rabbitmq -n YOUR_NAMESPACE manage CLUSTER_NAME
```

Running/stopping performance test:

```bash
kubectl rabbitmq -n YOUR_NAMESPACE perf-test CLUSTER_NAME

kubectl delete pod perf-test -n YOUR_NAMESPACE
kubectl delete service perf-test -n YOUR_NAMESPACE
```

## Commands

| Command                                                             | Description                                   |
| ------------------------------------------------------------------- | --------------------------------------------- |
| kubectl rabbitmq -n YOUR_NAMESPACE create CLUSTER_NAME --replicas 1 | Create cluster                                |
| kubectl rabbitmq delete CLUSTER_NAME                                | Delete cluster                                |
| kubectl rabbitmq get CLUSTER_NAME                                   | Display all resources associated with cluster |
| kubectl rabbitmq list                                               | List all clusters                             |
| kubectl rabbitmq secrets CLUSTER_NAME                               | Display default user secrets                  |
