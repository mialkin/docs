# Kubernetes

## kubectl

The **kubectl** command line tool lets you control Kubernetes clusters.

**Cluster**

Command                          | Description
---------------------------------|-----------------------
kubectl get nodes                | Show nodes in the cluster
kubectl get gateway              | Show gateways
kubectl get virtualservice       | Show virtual services
kubectl delete -f aspnetcore-gateway.yaml        | Delete gateway
Kubectl delete -f aspnetcore-virtualservice.yaml | Delete virtual service
kubectl delete -f aspnetcore.yaml                | Delete deployment and service

**Deployments**

Command                                       | Description
----------------------------------------------|-------------
kubectl get deployments                       | Display deployments
kubectl apply -f deployment.yaml              | Create deployment from yaml file
kubectl describe deployment \<deployment name>| Get datails of deployment
kubectl describe deployments                  | Get details of all deployment
kubectl delete deployment \<deployment name>  | Delete deployment

**Namespaces**

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.

Command                                       | Description
----------------------------------------------|-------------
kubectl get namespaces                        | List the current namespaces in a cluster
kubectl create ns \<namespace name>           | Create a new namespace

**Services**

Command                                | Description
---------------------------------------|------------------
kubectl get services                   |
kubectl delete service \<service name> | Delete service

**Pods**

Command                          | Description
---------------------------------|-----------------------
kubectl get pods                 |

**ReplicaSet**

Command                          | Description
---------------------------------|-----------------------
kubectl get rs                   | 

## kubeadm

**Commands**

Command                                        | Description
-----------------------------------------------|-----------------------
kubeadm token create --print-join-command      | Print out the join comman

## Links

* [↑ Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
* [↑ How to Deploy a Kubernetes Cluster with Ubuntu Server 18.04](https://thenewstack.io/how-to-deploy-a-kubernetes-cluster-with-ubuntu-server-18-04/)
* [↑ How To Create a Kubernetes Cluster Using Kubeadm on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-ubuntu-18-04)
* [↑ Container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
