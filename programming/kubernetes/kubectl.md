# kubectl

The **kubectl** command line tool lets you control Kubernetes clusters.

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.

Command                                          | Description
-------------------------------------------------|-----------------------
kubeadm token create --print-join-command        | Print out the join command
kubectl apply -f deployment.yaml                 | Create deployment
kubectl config current-context                   | Display the current context
kubectl config delete-context CONTEXT_NAME       | Delete context
kubectl config get-contexts                      | List all contexts (i.e. clusters)
kubectl config use-context CONTEXT_NAME          | Switch to context
kubectl create ns NAMESPACE_NAME                 | Create a new namespace
kubectl delete -f gateway.yaml                   | Delete gateway
kubectl delete -f virtual-service.yaml           | Delete virtual service
kubectl delete -f deployment-service.yaml        | Delete deployment and service
kubectl delete deployment DEPLOYMENT_NAME        | Delete deployment
kubectl delete service SERVICE_NAME              | Delete service
kubectl describe deployment DEPLOYMENT_NAME      | Get datails of deployment
kubectl describe deployments                     | Get details of all deployment
kubectl get deployments                          | Display deployments
kubectl get gateway                              | Show gateways
kubectl get namespaces                           | List the current namespaces in a cluster
kubectl get nodes                                | List nodes in the cluster
kubectl get pods                                 | List pods in the cluster
kubectl get rs                                   | Get replica sets
kubectl get services                             | List all services in the namespace
kubectl get virtualservice                       | Show virtual services

## Links

* [↑ kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [↑ How To Create a Kubernetes Cluster Using Kubeadm on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-ubuntu-18-04)
* [↑ Container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
