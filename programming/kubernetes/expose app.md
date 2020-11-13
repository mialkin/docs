# Expose your app publicly

In this scenario you will learn how to expose Kubernetes applications outside the cluster using the kubectl expose command. You will also learn how to view and apply labels to objects with the kubectl label command.

## Create a new service

Let’s list the current Services from our cluster:

```bash
kubectl get services
```

Output:

```text
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   24s
```

We have a Service called kubernetes that is created by default when minikube starts the cluster.

To create a new service and expose it to external traffic we’ll use the `expose` command with NodePort as parameter (minikube does not support the LoadBalancer option yet):

```bash
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

Let’s run again the `get services` command:

```bash
kubectl get services
```

Output:

```text
NAME                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes            ClusterIP   10.96.0.1       <none>        443/TCP          7m6s
kubernetes-bootcamp   NodePort    10.96.139.222   <none>        8080:32658/TCP   2s
```

We have now a running Service called kubernetes-bootcamp. Here we see that the Service received a unique cluster-IP, an internal port and an external-IP (the IP of the Node).

To find out what port was opened externally (by the NodePort option) we’ll run the `describe service` command:

```bash
kubectl describe services/kubernetes-bootcamp
```

Output:

```text
Name:                     kubernetes-bootcamp
Namespace:                default
Labels:                   run=kubernetes-bootcamp
Annotations:              <none>
Selector:                 run=kubernetes-bootcamp
Type:                     NodePort
IP:                       10.96.139.222
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  32658/TCP
Endpoints:                172.18.0.3:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

Create an environment variable called NODE_PORT that has the value of the Node port assigned:

```bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```

Output:

```text
NODE_PORT=32658
```

Now we can test that the app is exposed outside of the cluster using curl, the IP of the Node and the externally exposed port:

```bash
curl $(minikube ip):$NODE_PORT
```

Output:

```text
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-lfg94 | v=1
```

And we get a response from the server. The Service is exposed.
