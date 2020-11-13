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

## Using labels

The Deployment created automatically a label for our Pod. With `describe deployment` command you can see the name of the label:

```bash
kubectl describe deployment
```

Output:

```text
Name:                   kubernetes-bootcamp
Namespace:              default
CreationTimestamp:      Fri, 13 Nov 2020 15:22:32 +0000
Labels:                 run=kubernetes-bootcamp
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               run=kubernetes-bootcamp
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
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
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   kubernetes-bootcamp-765bf4c7b4 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  30m   deployment-controller  Scaled up replica set kubernetes-bootcamp-765bf4c7b4 to 1
```

Let’s use this label to query our list of Pods. We’ll use the `kubectl get pods` command with `-l` as a parameter, followed by the label values:

```bash
kubectl get pods -l run=kubernetes-bootcamp
```

Output:

```text
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-xsrfp   1/1     Running   0          5m16s
```

You can do the same to list the existing services:

```bash
kubectl get services -l run=kubernetes-bootcamp
```

Output:

```text
NAME                  TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes-bootcamp   NodePort   10.106.37.32   <none>        8080:30657/TCP   3m16s
```

You can do the same to list the existing services:

```bash
kubectl get services -l run=kubernetes-bootcamp
```

Get the name of the Pod and store it in the POD_NAME environment variable:

```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```

To apply a new label we use the label command followed by the object type, object name and the new label:

```bash
kubectl label pod $POD_NAME app=v1
```

Output:

```text
pod/kubernetes-bootcamp-765bf4c7b4-xsrfp labeled
```

This will apply a new label to our Pod (we pinned the application version to the Pod), and we can check it with the describe pod command:

```bash
kubectl describe pods $POD_NAME
```

Output:

```text
Name:         kubernetes-bootcamp-765bf4c7b4-xsrfp
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.50
Start Time:   Fri, 13 Nov 2020 20:34:39 +0000
Labels:       app=v1
              pod-template-hash=765bf4c7b4
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.3
IPs:  IP:           172.18.0.3
Controlled By:  ReplicaSet/kubernetes-bootcamp-765bf4c7b4Containers:
  kubernetes-bootcamp:    Container ID:   docker://1545c6e436cbca3cd54e3fe1124a2551be76a8b127e3c6e0ec04595ca366e784    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 20:34:41 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-67x6p (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-67x6p:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-67x6p
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age                From               Message
  ----     ------            ----               ----               -------
  Warning  FailedScheduling  12m (x2 over 12m)  default-scheduler  0/1 nodes are available: 1 node(s) had taints that the pod didn't tolerate.
  Normal   Scheduled         12m                default-scheduler  Successfully assigned default/kubernetes-bootcamp-765bf4c7b4-xsrfp to minikube
  Normal   Pulled            12m                kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
  Normal   Created           12m                kubelet, minikube  Created container kubernetes-bootcamp
  Normal   Started           12m                kubelet, minikube  Started container kubernetes-bootcamp
```

We see here that the label is attached now to our Pod. And we can query now the list of pods using the new label:

```bash
kubectl get pods -l app=v1
```

Output:

```text
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-xsrfp   1/1     Running   0          13m
```

And we see the Pod.

## Deleting a service

To delete Services you can use the delete service command. Labels can be used also here:

```bash
kubectl delete service -l run=kubernetes-bootcamp
```

Confirm that the service is gone:

```bash
kubectl get services
```

This confirms that our Service was removed. To confirm that route is not exposed anymore you can curl the previously exposed IP and port:

```bash
curl $(minikube ip):$NODE_PORT
```

This proves that the app is not reachable anymore from outside of the cluster. You can confirm that the app is still running with a curl inside the pod:

```bash
kubectl exec -ti $POD_NAME curl localhost:8080
```

We see here that the application is up. This is because the Deployment is managing the application. To shut down the application, you would need to delete the Deployment as well.
