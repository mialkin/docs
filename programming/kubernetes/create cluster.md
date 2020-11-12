# Create Kubernetes cluster

The goal of this interactive scenario is to deploy a local development Kubernetes cluster using minikube.

## Starting up cluster

We already installed minikube for you. Check that it is properly installed, by running:

```bash
minikube version
```

Start the cluster:

```bash
minikube start
```

## Cluster version

To interact with Kubernetes we’ll use the command line interface, `kubectl`. To check if kubectl is installed you can run:

```bash
kubectl version
```

OK, kubectl is configured and we can see both the version of the client and as well as the server. The client version is the kubectl version; the server version is the Kubernetes version installed on the master. You can also see details about the build:

```text
"Client Version":"version.Info"{
   "Major":"1",
   "Minor":"17",
   "GitVersion":"v1.17.0",
   "GitCommit":"70132b0f130acc0bed193d9ba59dd186f0e634cf",
   "GitTreeState":"clean",
   "BuildDate":"2019-12-07T21:20:10Z",
   "GoVersion":"go1.13.4",
   "Compiler":"gc",
   "Platform":"linux/amd64"
}"Server Version":"version.Info"{
   "Major":"1",
   "Minor":"17",
   "GitVersion":"v1.17.0",
   "GitCommit":"70132b0f130acc0bed193d9ba59dd186f0e634cf",
   "GitTreeState":"clean",
   "BuildDate":"2019-12-07T21:12:17Z",
   "GoVersion":"go1.13.4",
   "Compiler":"gc",
   "Platform":"linux/amd64"
}
```

## Cluster details

Let’s view the cluster details:

```bash
kubectl cluster-info
```

To view the nodes in the cluster, run:

```bash
kubectl get nodes
```

This command shows all nodes that can be used to host our applications. Now we have only one node, and we can see that its status is ready (it is ready to accept applications for deployment):

```text
NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    master   10m   v1.17.0
```
