# Kubernetes

## kubectl

The **kubectl** command line tool lets you control Kubernetes clusters.

### Syntax

```text
kubectl [command] [TYPE] [NAME] [flags]
```

### Commands

Command          | Description
-----------------|-----------------------
kubectl get      | List resources
kubectl describe | Show detailed information about a resource
kubectl logs     | Print the logs from a container in a pod
kubectl exec     | Execute a command on a container in a pod

## kubeadm

### Commands

Command                                        | Description
-----------------------------------------------|-----------------------
kubeadm token create --print-join-command      | Print out the join comman

## Links

* [↑ Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
* [↑ How to Deploy a Kubernetes Cluster with Ubuntu Server 18.04](https://thenewstack.io/how-to-deploy-a-kubernetes-cluster-with-ubuntu-server-18-04/)
* [↑ How To Create a Kubernetes Cluster Using Kubeadm on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-ubuntu-18-04)
* [↑ Container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
