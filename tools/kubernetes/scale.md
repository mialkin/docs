# Scale up your app

To list your deployments use the get deployments command: 

```bash
kubectl get deployments
```

The output should be similar to:

```text
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   1/1     1            1           11m
```

We should have 1 Pod. If not, run the command again. This shows:

* NAME lists the names of the Deployments in the cluster.
* READY shows the ratio of CURRENT/DESIRED replicas
* UP-TO-DATE displays the number of replicas that have been updated to achieve the desired state.
* AVAILABLE displays how many replicas of the application are available to your users.
* AGE displays the amount of time that the application has been running.

To see the ReplicaSet created by the Deployment, run:

```bash
kubectl get rs
```

Output:

```text
NAME                             DESIRED   CURRENT   READY   AGE
kubernetes-bootcamp-765bf4c7b4   1         1         1       2m49s
```

Notice that the name of the ReplicaSet is always formatted as [DEPLOYMENT-NAME]-[RANDOM-STRING]. The random string is randomly generated and uses the pod-template-hash as a seed.

Two important columns of this command are:

* DESIRED displays the desired number of replicas of the application, which you define when you create the Deployment. This is the desired state.
* CURRENT displays how many replicas are currently running.

Next, let’s scale the Deployment to 4 replicas. We’ll use the `kubectl scale` command, followed by the deployment type, name and desired number of instances:

```bash
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

To list your Deployments once again, use get deployments:

```bash
kubectl get deployments
```

Output:

```text
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   4/4     4            4           18m
```

The change was applied, and we have 4 instances of the application available. Next, let’s check if the number of Pods changed:

```bash
kubectl get pods -o wide
```

Output:

```text
NAME                                   READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
kubernetes-bootcamp-765bf4c7b4-46kkp   1/1     Running   0          36s   172.18.0.8   minikube   <none>           <none>
kubernetes-bootcamp-765bf4c7b4-6sk9h   1/1     Running   0          36s   172.18.0.7   minikube   <none>           <none>
kubernetes-bootcamp-765bf4c7b4-hf9k7   1/1     Running   0          36s   172.18.0.9   minikube   <none>           <none>
kubernetes-bootcamp-765bf4c7b4-l6snc   1/1     Running   0          18m   172.18.0.3   minikube   <none>           <none>
```

There are 4 Pods now, with different IP addresses. The change was registered in the Deployment events log. To check that, use the describe command:

```bash
kubectl describe deployments/kubernetes-bootcamp
```

Output:

```text
Name:                   kubernetes-bootcamp
Namespace:              default
CreationTimestamp:      Fri, 13 Nov 2020 20:59:21 +0000
Labels:                 run=kubernetes-bootcamp
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               run=kubernetes-bootcamp
Replicas:               4 desired | 4 updated | 4 total | 4 available | 0unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  run=kubernetes-bootcamp
  Containers:
   kubernetes-bootcamp:
    Image:        gcr.io/google-samples/kubernetes-bootcamp:v1
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   kubernetes-bootcamp-765bf4c7b4 (4/4 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  21m    deployment-controller  Scaled up replica set kubernetes-bootcamp-765bf4c7b4 to 1
  Normal  ScalingReplicaSet  3m38s  deployment-controller  Scaled up replica set kubernetes-bootcamp-765bf4c7b4 to 4
```

You can also view in the output of this command that there are 4 replicas now.

## Load balancing

Let’s check that the Service is load-balancing the traffic. To find out the exposed IP and Port we can use the describe service as we learned in the previous Module:

```bash
kubectl describe services/kubernetes-bootcamp
```

Ouput:

```text
Name:                     kubernetes-bootcamp
Namespace:                default
Labels:                   run=kubernetes-bootcamp
Annotations:              <none>
Selector:                 run=kubernetes-bootcamp
Type:                     NodePort
IP:                       10.111.140.123
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  32436/TCP
Endpoints:                172.18.0.3:8080,172.18.0.7:8080,172.18.0.8:8080 + 1 more...
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

Create an environment variable called NODE_PORT that has a value as the Node port:

```bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```

Next, we’ll do a curl to the exposed IP and port. Execute the command multiple times:

```bash
curl $(minikube ip):$NODE_PORT
curl $(minikube ip):$NODE_PORT
curl $(minikube ip):$NODE_PORT
curl $(minikube ip):$NODE_PORT
curl $(minikube ip):$NODE_PORT
curl $(minikube ip):$NODE_PORT
```

Output:

```text
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-l6snc | v=1
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-6sk9h | v=1
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-46kkp | v=1
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-hf9k7 | v=1
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-hf9k7 | v=1
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-46kkp | v=1
```

We hit a different Pod with every request. This demonstrates that the load-balancing is working.

## Scale Down

To scale down the Service to 2 replicas, run again the scale command:

```bash
kubectl scale deployments/kubernetes-bootcamp --replicas=2
```

Output:

```text
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   2/2     2            2           36m
```

List the Deployments to check if the change was applied with the get deployments command:

```bash
kubectl get deployments
```

The number of replicas decreased to 2. List the number of Pods, with get pods:

```bash
kubectl get pods -o wide
```

Output:

```text
NAME                                   READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
kubernetes-bootcamp-765bf4c7b4-46kkp   1/1     Running   0          20m   172.18.0.8   minikube   <none>           <none>
kubernetes-bootcamp-765bf4c7b4-l6snc   1/1     Running   0          38m   172.18.0.3   minikube   <none>           <none>
```

This confirms that 2 Pods were terminated.