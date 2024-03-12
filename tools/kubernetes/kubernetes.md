# Kubernetes

[↑ Kubernetes](https://kubernetes.io) is an open-source system for automating deployment, scaling, and management of containerized applications.

## Table of contents

- [Kubernetes](#kubernetes)
  - [Table of contents](#table-of-contents)
  - [Cluster role](#cluster-role)
  - [Cluster role binding](#cluster-role-binding)
  - [Configmap](#configmap)
  - [Cron job](#cron-job)
  - [Deployment](#deployment)
  - [Ingress](#ingress)
  - [Ingress resource](#ingress-resource)
  - [Ingress controller](#ingress-controller)
  - [Job](#job)
  - [Namespace](#namespace)
  - [Pod](#pod)
  - [Replica set](#replica-set)
  - [Role](#role)
  - [Role binding](#role-binding)
  - [Secret](#secret)
  - [Service](#service)
  - [Service account](#service-account)

## Cluster role

A **cluster role** is an object that sets cluster-wide permissions.

If you want to define a role within a namespace, use a [role](#role).

[↑ Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).

## Cluster role binding

A **cluster role binding** is an object that grants permissions cluster-wide.

A [role binding](#role-binding) grants permissions within a specified namespace.

## Configmap

A **configmap** is an object used to store non-confidential data in key-value pairs. [Pods](#pod) can consume configmap as environment variables, command-line arguments, or as configuration files in a volume.

[↑ ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap).

## Cron job

A **cron job** creates [jobs](#job) on a repeating schedule.

Cron job is meant for performing regular scheduled actions such as backups, report generation, and so on. One `CronJob` object is like one line of a [crontab](/unix/tools/crontab.md) file on a Unix system. It runs a job periodically on a given schedule, written in cron format.

[↑ CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/).

## Deployment

A **deployment** is an object that provides declarative updates for [pods](#pod) and [replica sets](#replica-set).

[↑ Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment).

## Ingress

An **ingress** is an object that manages external access to the services in a cluster, typically HTTP.

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the [ingress resource](#ingress-resource).

Ingress may provide load balancing, SSL termination and name-based virtual hosting.

[↑ Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress).

## Ingress resource

An **ingress resource** is an object with a set of routing rules.

## Ingress controller

An **ingress controller** is just another [pod](#pod) running in Kubernetes that is responsible for reading the [ingress resource](#ingress-resource) information and processing that data accordingly.

[↑ Ingress Controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers).

## Job

A **job** creates one or more [pods](#pod) and will continue to retry execution of the pods until a specified number of them successfully terminate.

[↑ Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/).

## Namespace

A **namespace** is an object that isolates groups of resources within a single cluster.

Names of resources need to be unique within a namespace, but not across namespaces.

Namespace-based scoping is applicable only for namespaced objects like `Deployment`, `Service`, and not for cluster-wide objects like `StorageClass`, `Node`, `PersistentVolumes`.

[↑ Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces).

## Pod

A **pod** is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.

[↑ Pod](https://kubernetes.io/docs/concepts/workloads/pods).

## Replica set

A **replica set** is an object that maintains a stable set of replica [pods](#pod) running at any given time.

As such, it is often used to guarantee the availability of a specified number of identical pods.

[↑ ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset).

## Role

A **role** is an object that sets permissions within a particular namespace; when you create a role, you have to specify the namespace it belongs to.

If you want to define a role cluster-wide, use a [cluster role](#cluster-role).

[↑ Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).

## Role binding

A **role binding** is an object that grants the permissions defined in a role to a user or set of users within a specific namespace.

Role binding holds a list of *subjects*: users, groups, or service accounts, and a reference to the role being granted.

A [cluster role binding](#cluster-role-binding) grants permissions cluster-wide.

## Secret

A **secret** is an object that contains a small amount of sensitive data such as a password, a token, or a key.

Secrets are similar to [configmaps](#configmap) but are specifically intended to hold confidential data.

[↑ Secrets](https://kubernetes.io/docs/concepts/configuration/secret).

## Service

A **service** is an object that defines a logical set of [pods](#pod) and a policy by which to access them.

[↑ Service](https://kubernetes.io/docs/concepts/services-networking/service).

## Service account

A **service account** is an object that provides an identity for processes that run in a [pod](#pod).

A process inside a pod can use the identity of its associated service account to authenticate to the cluster's API server.

Service accounts are tied to a set of credentials stored as [secrets](#secret), which are mounted into pods allowing in-cluster processes to talk to the Kubernetes API.

Service accounts are bound to specific namespaces, and created automatically by the API server or manually through API calls.

[↑ Managing Service Accounts](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin).

[↑ Authenticating](https://kubernetes.io/docs/reference/access-authn-authz/authentication/).
