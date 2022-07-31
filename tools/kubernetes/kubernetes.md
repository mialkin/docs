# Kubernetes

## Table of contents

- [Kubernetes](#kubernetes)
  - [Table of contents](#table-of-contents)
  - [Ingress resource](#ingress-resource)
    - [Ingress controller](#ingress-controller)
  - [`ClusterIP`, `NodePort`, `LoadBalancer`](#clusterip-nodeport-loadbalancer)

## Ingress resource

An **ingress resource** is an object with a set of routing rules.

### Ingress controller

An **ingress controller** is just another pod running in Kubernetes.

The ingress controller is responsible for reading the ingress resource information and processing that data accordingly.

## `ClusterIP`, `NodePort`, `LoadBalancer`

[↑ StackOverflow answer](https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types)
