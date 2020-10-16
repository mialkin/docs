# Kubernetes

**Kubernetes** is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications.

The name Kubernetes originates from Greek, meaning helmsman or pilot. Google open-sourced the Kubernetes project in 2014. Kubernetes combines over 15 years of Google's experience running production workloads at scale with best-of-breed ideas and practices from the community.

## Nodes

A Kubernetes cluster consists of a set of worker machines, called **nodes**, that run containerized applications. Every cluster has at least one worker node.

## Pods

The worker node(s) host the pods.

A **pod** is a set of running containers in your cluster.

## Control plane

The control plane manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.

The **control plane** is a container orchestration layer that exposes the API and interfaces to define, deploy and manage the lifecycle of containers

Control plane components can be run on any machine in the cluster. However, for simplicity, set up scripts typically start all control plane components on the same machine, and do not run user containers on this machine.
