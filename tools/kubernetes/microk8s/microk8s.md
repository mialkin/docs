# microk8s

## Installation

Installation [↑ instruction](https://ubuntu.com/kubernetes/install):

```bash
sudo snap install microk8s --classic
microk8s status --wait-ready
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
```

Print out `.kube/config` file:

```bash
microk8s config
```

Turn on services you need:

```bash
microk8s enable dns ingress dashboard prometheus
```

See installed addons:

```bash
microk8s status
```

## Run dashboard

```bash
kubectl proxy
# http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy
```

## MetalLB

Enable [↑ MetaLB](https://metallb.universe.tf):

```bash
microk8s enable metallb
```

Use IP range: `192.168.0.100-192.168.0.150`.

> Your WiFi router should include this range in its DHCP server's range: `DHCP Start IP Address`/`DHCP End IP Address`.

Make sure that a service got external IP:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 4
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  externalTrafficPolicy: Local
  type: LoadBalancer
```
