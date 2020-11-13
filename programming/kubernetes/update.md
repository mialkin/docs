# Update your app

Update the version of the app
To list your deployments use the get deployments command: kubectl get deployments

To list the running Pods use the get pods command:

```bash
kubectl get pods
```

Output:

```text
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-ghngh   1/1     Running   0          6m41s
kubernetes-bootcamp-765bf4c7b4-lqsz7   1/1     Running   0          6m41s
kubernetes-bootcamp-765bf4c7b4-rttv9   1/1     Running   0          6m41s
kubernetes-bootcamp-765bf4c7b4-zvpcp   1/1     Running   0          6m41s
```

To view the current image version of the app, run a describe command against the Pods (look at the Image field):

```bash
kubectl describe pods
```

Output:

```text
Name:         kubernetes-bootcamp-765bf4c7b4-ghngh
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.14
Start Time:   Fri, 13 Nov 2020 21:55:35 +0000
Labels:       pod-template-hash=765bf4c7b4
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.9
IPs:
  IP:           172.18.0.9
Controlled By:  ReplicaSet/kubernetes-bootcamp-765bf4c7b4
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://764f5592f71f90beea71362b0c3b3200b807630e4834b6bf92ff753c65a3bfb7
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 21:55:40 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-d5htj (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-d5htj:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-d5htj
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  7m16s  default-scheduler  Successfully assigned default/kubernetes-bootcamp-765bf4c7b4-ghngh to minikube
  Normal  Pulled     7m12s  kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
  Normal  Created    7m12s  kubelet, minikube  Created container kubernetes-bootcamp
  Normal  Started    7m11s  kubelet, minikube  Started container kubernetes-bootcamp


Name:         kubernetes-bootcamp-765bf4c7b4-lqsz7
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.14
Start Time:   Fri, 13 Nov 2020 21:55:35 +0000
Labels:       pod-template-hash=765bf4c7b4
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.6
IPs:
  IP:           172.18.0.6
Controlled By:  ReplicaSet/kubernetes-bootcamp-765bf4c7b4
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://1e7206b2d6e2da2c3922132477fc1708854f434d02021b32571b0b98058abbfb
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 21:55:39 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-d5htj (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-d5htj:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-d5htj
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  7m16s  default-scheduler  Successfully assigned default/kubernetes-bootcamp-765bf4c7b4-lqsz7 to minikube
  Normal  Pulled     7m13s  kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
  Normal  Created    7m13s  kubelet, minikube  Created container kubernetes-bootcamp
  Normal  Started    7m12s  kubelet, minikube  Started container kubernetes-bootcamp


Name:         kubernetes-bootcamp-765bf4c7b4-rttv9
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.14
Start Time:   Fri, 13 Nov 2020 21:55:35 +0000
Labels:       pod-template-hash=765bf4c7b4
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.7
IPs:
  IP:           172.18.0.7
Controlled By:  ReplicaSet/kubernetes-bootcamp-765bf4c7b4
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://5e1426051f194042073e9885460cffcbb642613beecaf5a65a0f3ee1f9d84583
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 21:55:39 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-d5htj (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-d5htj:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-d5htj
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  7m16s  default-scheduler  Successfully assigned default/kubernetes-bootcamp-765bf4c7b4-rttv9 to minikube
  Normal  Pulled     7m13s  kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
  Normal  Created    7m13s  kubelet, minikube  Created container kubernetes-bootcamp
  Normal  Started    7m12s  kubelet, minikube  Started container kubernetes-bootcamp


Name:         kubernetes-bootcamp-765bf4c7b4-zvpcp
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.14
Start Time:   Fri, 13 Nov 2020 21:55:35 +0000
Labels:       pod-template-hash=765bf4c7b4
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.8
IPs:
  IP:           172.18.0.8
Controlled By:  ReplicaSet/kubernetes-bootcamp-765bf4c7b4
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://f6856bba13a8b47712b9fac4a91a7f88499fabc3ec4158b7d10bc29e1f201422
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 21:55:40 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-d5htj (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-d5htj:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-d5htj
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  7m16s  default-scheduler  Successfully assigned default/kubernetes-bootcamp-765bf4c7b4-zvpcp to minikube
  Normal  Pulled     7m12s  kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
  Normal  Created    7m12s  kubelet, minikube  Created container kubernetes-bootcamp
  Normal  Started    7m11s  kubelet, minikube  Started container kubernetes-bootcamp
```

To update the image of the application to version 2, use the set image command, followed by the deployment name and the new image version:

```bash
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```

The command notified the Deployment to use a different image for your app and initiated a rolling update. Check the status of the new Pods, and view the old one terminating with the get pods command:

```bash
kubectl get pods
```

Output:

```text
NAME                                   READY   STATUS              RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-ghngh   1/1     Terminating         0          8m24s
kubernetes-bootcamp-765bf4c7b4-lqsz7   1/1     Terminating         0          8m24s
kubernetes-bootcamp-765bf4c7b4-rttv9   1/1     Terminating         0          8m24s
kubernetes-bootcamp-765bf4c7b4-zvpcp   1/1     Running             0          8m24s
kubernetes-bootcamp-7d6f8694b6-8l46t   0/1     ContainerCreating   0          1s
kubernetes-bootcamp-7d6f8694b6-bsrms   1/1     Running             0          4s
kubernetes-bootcamp-7d6f8694b6-lfj5m   1/1     Running             0          4s
kubernetes-bootcamp-7d6f8694b6-r9b5v   0/1     ContainerCreating   0          1s
```

## Verify an update

First, let’s check that the App is running. To find out the exposed IP and Port we can use describe service:

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
IP:                       10.104.133.58
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  32474/TCP
Endpoints:                172.18.0.10:8080,172.18.0.11:8080,172.18.0.12:8080 + 1 more...
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

Create an environment variable called NODE_PORT that has the value of the Node port assigned:

```bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```

Next, we’ll do a curl to the the exposed IP and port:

```bash
curl $(minikube ip):$NODE_PORT
```

Output:

```text
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-7d6f8694b6-r9b5v | v=2
```

We hit a different Pod with every request and we see that all Pods are running the latest version (v2).

The update can be confirmed also by running a rollout status command:

```bash
kubectl rollout status deployments/kubernetes-bootcamp
```

Output:

```text
deployment "kubernetes-bootcamp" successfully rolled out
```

To view the current image version of the app, run a describe command against the Pods:

```bash
kubectl describe pods
```

Output:

```text
Name:         kubernetes-bootcamp-7d6f8694b6-8l46t
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.14
Start Time:   Fri, 13 Nov 2020 22:03:58 +0000
Labels:       pod-template-hash=7d6f8694b6
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.13
IPs:
  IP:           172.18.0.13
Controlled By:  ReplicaSet/kubernetes-bootcamp-7d6f8694b6
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://d41ed55759fb0c5023defc3f9142f2f14a1c6eb8f1ab80f32c3ac284521ca1ff
    Image:          jocatalin/kubernetes-bootcamp:v2
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:fb1a3ced00cecfc1f83f18ab5cd14199e30adc1b49aa4244f5d65ad3f5feb2a5
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 22:04:01 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-d5htj (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-d5htj:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-d5htj
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  18m   default-scheduler  Successfully assigned default/kubernetes-bootcamp-7d6f8694b6-8l46t to minikube
  Normal  Pulled     18m   kubelet, minikube  Container image "jocatalin/kubernetes-bootcamp:v2" already present on machine
  Normal  Created    18m   kubelet, minikube  Created container kubernetes-bootcamp
  Normal  Started    18m   kubelet, minikube  Started container kubernetes-bootcamp


Name:         kubernetes-bootcamp-7d6f8694b6-bsrms
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.14
Start Time:   Fri, 13 Nov 2020 22:03:55 +0000
Labels:       pod-template-hash=7d6f8694b6
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.11
IPs:
  IP:           172.18.0.11
Controlled By:  ReplicaSet/kubernetes-bootcamp-7d6f8694b6
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://969cbcac57a8dd10b762d58d36c5ce87c6091fcfd1ee73f6f0c170edf93b9a6a
    Image:          jocatalin/kubernetes-bootcamp:v2
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:fb1a3ced00cecfc1f83f18ab5cd14199e30adc1b49aa4244f5d65ad3f5feb2a5
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 22:03:57 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-d5htj (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-d5htj:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-d5htj
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  18m   default-scheduler  Successfully assigned default/kubernetes-bootcamp-7d6f8694b6-bsrms to minikube
  Normal  Pulled     18m   kubelet, minikube  Container image "jocatalin/kubernetes-bootcamp:v2" already present on machine
  Normal  Created    18m   kubelet, minikube  Created container kubernetes-bootcamp
  Normal  Started    18m   kubelet, minikube  Started container kubernetes-bootcamp


Name:         kubernetes-bootcamp-7d6f8694b6-lfj5m
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.14
Start Time:   Fri, 13 Nov 2020 22:03:55 +0000
Labels:       pod-template-hash=7d6f8694b6
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.10
IPs:
  IP:           172.18.0.10
Controlled By:  ReplicaSet/kubernetes-bootcamp-7d6f8694b6
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://8d43b5e4f21a5ea867b95f32f5de48244b87ab1eb4588e23f575759e6bb86d1c
    Image:          jocatalin/kubernetes-bootcamp:v2
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:fb1a3ced00cecfc1f83f18ab5cd14199e30adc1b49aa4244f5d65ad3f5feb2a5
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 22:03:57 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-d5htj (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-d5htj:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-d5htj
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  18m   default-scheduler  Successfully assigned default/kubernetes-bootcamp-7d6f8694b6-lfj5m to minikube
  Normal  Pulled     18m   kubelet, minikube  Container image "jocatalin/kubernetes-bootcamp:v2" already present on machine
  Normal  Created    18m   kubelet, minikube  Created container kubernetes-bootcamp
  Normal  Started    18m   kubelet, minikube  Started container kubernetes-bootcamp


Name:         kubernetes-bootcamp-7d6f8694b6-r9b5v
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.14
Start Time:   Fri, 13 Nov 2020 22:03:58 +0000
Labels:       pod-template-hash=7d6f8694b6
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.12
IPs:
  IP:           172.18.0.12
Controlled By:  ReplicaSet/kubernetes-bootcamp-7d6f8694b6
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://8c73d7398ce908d3e1cd6274d5a7d06cf41716308aa3d7243f1e12740437b20d
    Image:          jocatalin/kubernetes-bootcamp:v2
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:fb1a3ced00cecfc1f83f18ab5cd14199e30adc1b49aa4244f5d65ad3f5feb2a5
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 22:04:00 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-d5htj (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-d5htj:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-d5htj
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  18m   default-scheduler  Successfully assigned default/kubernetes-bootcamp-7d6f8694b6-r9b5v to minikube
  Normal  Pulled     18m   kubelet, minikube  Container image "jocatalin/kubernetes-bootcamp:v2" already present on machine
  Normal  Created    18m   kubelet, minikube  Created container kubernetes-bootcamp
  Normal  Started    18m   kubelet, minikube  Started container kubernetes-bootcamp
```

We run now version 2 of the app (look at the Image field).

## Rollback an update

Let’s perform another update, and deploy image tagged as v10:

```bash
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10
```

Use get deployments to see the status of the deployment:

```bash
kubectl get deployments
```

And something is wrong… We do not have the desired number of Pods available. List the Pods again:

```bash
kubectl get pods
```

A describe command on the Pods should give more insights:

```bash
kubectl describe pods
```

There is no image called v10 in the repository. Let’s roll back to our previously working version. We’ll use the rollout undo command:

```bash
kubectl rollout undo deployments/kubernetes-bootcamp
```

The rollout command reverted the deployment to the previous known state (v2 of the image). Updates are versioned and you can revert to any previously know state of a Deployment. List again the Pods:

```bash
kubectl get pods
```

Four Pods are running. Check again the image deployed on the them:

```bash
kubectl describe pods
```

We see that the deployment is using a stable version of the app (v2). The Rollback was successful.
