# Kubernetes

## Table of contents

- [Kubernetes](#kubernetes)
  - [Table of contents](#table-of-contents)
  - [Configmap](#configmap)
  - [Deployment](#deployment)
  - [Ingress](#ingress)
  - [Ingress resource](#ingress-resource)
  - [Ingress controller](#ingress-controller)
  - [Namespace](#namespace)
  - [Pod](#pod)
  - [Replicaset](#replicaset)
  - [Secret](#secret)
  - [Service](#service)
  - [`ClusterIP`, `NodePort`, `LoadBalancer`](#clusterip-nodeport-loadbalancer)

## Configmap

A **configmap** is an object used to store non-confidential data in key-value pairs. [Pods](#pod) can consume configmap as environment variables, command-line arguments, or as configuration files in a volume.

[↑ ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap)

## Deployment

A **deployment** is an object that provides declarative updates for [pods](#pod) and [replicasets](#replicaset).

[↑ Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment)

## Ingress

An **ingress** is an object that manages external access to the services in a cluster, typically HTTP.

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the [ingress resource](#ingress-resource).

Ingress may provide load balancing, SSL termination and name-based virtual hosting.

[↑ Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress)

## Ingress resource

An **ingress resource** is an object with a set of routing rules.

## Ingress controller

An **ingress controller** is just another [pod](#pod) running in Kubernetes that is responsible for reading the [ingress resource](#ingress-resource) information and processing that data accordingly.

[↑ Ingress Controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers)

## Namespace

A **namespace** is an object that isolates groups of resources within a single cluster.

Names of resources need to be unique within a namespace, but not across namespaces.

Namespace-based scoping is applicable only for namespaced objects like `Deployment`, `Service`, and not for cluster-wide objects like `StorageClass`, `Node`, `PersistentVolumes`.

[↑ Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces)

## Pod

A **pod** is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.

[↑ Pod](https://kubernetes.io/docs/concepts/workloads/pods)

## Replicaset

A **replicaset** is an object that maintains a stable set of replica [pods](#pod) running at any given time.

As such, it is often used to guarantee the availability of a specified number of identical pods.

[↑ ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset)

## Secret

A **secret** is an object that contains a small amount of sensitive data such as a password, a token, or a key.

Secrets are similar to [configmaps](#configmap) but are specifically intended to hold confidential data.

[↑ Secrets](https://kubernetes.io/docs/concepts/configuration/secret)

## Service

A **service** is an object that defines a logical set of [pods](#pod) and a policy by which to access them.

[↑ Service](https://kubernetes.io/docs/concepts/services-networking/service)

## `ClusterIP`, `NodePort`, `LoadBalancer`

[↑ StackOverflow answer](https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types)