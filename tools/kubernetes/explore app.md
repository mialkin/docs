# Explore your app

## Kubernetes Pods

A Pod is a Kubernetes abstraction that represents a group of one or more application containers (such as Docker), and some shared resources for those containers. Those resources include:

Shared storage, as Volumes
Networking, as a unique cluster IP address
Information about how to run each container, such as the container image version or specific ports to use
A Pod models an application-specific "logical host" and can contain different application containers which are relatively tightly coupled. For example, a Pod might include both the container with your Node.js app as well as a different container that feeds the data to be published by the Node.js webserver. The containers in a Pod share an IP Address and port space, are always co-located and co-scheduled, and run in a shared context on the same Node.

Pods are the atomic unit on the Kubernetes platform. When we create a Deployment on Kubernetes, that Deployment creates Pods with containers inside them (as opposed to creating containers directly). Each Pod is tied to the Node where it is scheduled, and remains there until termination (according to restart policy) or deletion. In case of a Node failure, identical Pods are scheduled on other available Nodes in the cluster.

## Nodes

A Pod always runs on a Node. A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster. Each Node is managed by the Master. A Node can have multiple pods, and the Kubernetes master automatically handles scheduling the pods across the Nodes in the cluster. The Master's automatic scheduling takes into account the available resources on each Node.

Every Kubernetes Node runs at least:

Kubelet, a process responsible for communication between the Kubernetes Master and the Node; it manages the Pods and the containers running on a machine.
A container runtime (like Docker) responsible for pulling the container image from a registry, unpacking the container, and running the application.

## Check application configuration

Let’s verify that the application we deployed in the previous scenario is running:

```bash
kubectl get pods
```

Output:

```text
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-pxw2t   1/1     Running   0          3m1
```

To view what containers are inside that Pod and what images are used to build those containers run:

```bash
kubectl describe pods
```

Output:

```text
Name:         kubernetes-bootcamp-765bf4c7b4-nk9k4
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.43
Start Time:   Fri, 13 Nov 2020 13:02:13 +0000
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
    Container ID:   docker://795ba96d9f6c90aaeb04da9e31b5fec0ca59cf50e8580d7873b054f132bd2374
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Fri, 13 Nov 2020 13:02:17 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-wl97x (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-wl97x:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-wl97x
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age                From               Message
  ----     ------            ----               ----               -------
  Warning  FailedScheduling  25m (x3 over 26m)  default-scheduler  0/1 nodes are available: 1 node(s) had taints that the pod didn't tolerate.
  Normal   Scheduled         25m                default-scheduler  Successfully assigned default/kubernetes-bootcamp-765bf4c7b4-nk9k4 to minikube
  Normal   Pulled            25m                kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
  Normal   Created           25m                kubelet, minikube  Created container kubernetes-bootcamp
  Normal   Started           25m                kubelet, minikube  Started container kubernetes-bootcamp
```

We see here details about the Pod’s container: IP address, the ports used and a list of events related to the lifecycle of the Pod.

> Note: the describe command can be used to get detailed information about most of the kubernetes primitives: node, pods, deployments. The describe output is designed to be human readable, not to be scripted against.

## Show the app in the terminal

Pods are running in an isolated, private network - so we need to proxy access to them so we can debug and interact with them.

Run a proxy in a second terminal window:

```bash
kubectl proxy
```

Now again, we'll get the Pod name and query that pod directly through the proxy. To get the Pod name and store it in the POD_NAME environment variable:

```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```

To see the output of our application, run a curl request.

```bash
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
```

Output:

```text
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-nk9k4 | v=1
```

The url is the route to the API of the Pod.

## View the container logs

Anything that the application would normally send to `STDOUT` becomes logs for the container within the Pod:

```bash
kubectl logs $POD_NAME
```

Output:

```text
Kubernetes Bootcamp App Started At: 2020-11-13T13:02:17.660Z | Running On:  kubernetes-bootcamp-765bf4c7b4-nk9k4

Running On: kubernetes-bootcamp-765bf4c7b4-nk9k4 | Total Requests: 1 | App Uptime: 1611.113 seconds | Log Time: 2020-11-13T13:29:08.773Z
```

> Note: We don’t need to specify the container name, because we only have one container inside the pod.

## Executing command on the container

We can execute commands directly on the container once the Pod is up and running.

Let’s list the environment variables:

```bash
kubectl exec $POD_NAME env
```

Output:

```text
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=kubernetes-bootcamp-765bf4c7b4-nk9k4
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PROTO=tcp
NPM_CONFIG_LOGLEVEL=info
NODE_VERSION=6.3.1
HOME=/root
```

Again, worth mentioning that the name of the container itself can be omitted since we only have a single container in the Pod.

Next let’s start a bash session in the Pod’s container:

```bash
kubectl exec -ti $POD_NAME bash
```

We have now an open console on the container where we run our NodeJS application. The source code of the app is in the server.js file:

```bash
cat server.js
```

Output:

```text
var http = require('http');
var requests=0;
var podname= process.env.HOSTNAME;
var startTime;
var host;
var handleRequest = function(request, response) {
  response.setHeader('Content-Type', 'text/plain');
  response.writeHead(200);
  response.write("Hello Kubernetes bootcamp! | Running on: ");
  response.write(host);
  response.end(" | v=1\n");
  console.log("Running On:" ,host, "| Total Requests:", ++requests,"| App Uptime:", (new Date() - startTime)/1000 , "seconds", "| Log Time:",new Date());
}
var www = http.createServer(handleRequest);
www.listen(8080,function () {
    startTime = new Date();;
    host = process.env.HOSTNAME;
    console.log ("Kubernetes Bootcamp App Started At:",startTime, "| Running On: " ,host, "\n" );
});
```

You can check that the application is up by running a curl command:

```bash
curl localhost:8080
```

Output:

```text
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-nk9k4 | v=1
```

To close your container connection type:

```bash
exit
```
