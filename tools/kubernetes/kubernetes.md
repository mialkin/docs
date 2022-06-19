# Kubernetes

## Ingress resource vs ingress controller

An **ingress resource** is an object with a set of routing rules.

An **ingress controller** is just another pod running in k8s.

The ingress controller is responsible for reading the ingress resource information and processing that data accordingly.

## Difference between ClusterIP, NodePort and LoadBalancer service types

[↑ StackOverflow answer](https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types)

## Your App Deserves More than Kubernetes Ingress: Kubernetes Ingress vs. Istio Gateway

Any application running at production scale should have an “Ingress” to expose itself to the outside world. While Kubernetes provides the “Ingress” resource for this purpose, its feature set is limited depending on the kind of Ingress Controller (usually nginx) being used. Alternatively, you can leverage Istio and take advantage of its more feature-rich Ingress Gateway resource, even if your application Pods themselves are not running purely Kubernetes. We can do so by incrementally adopting Istio’s feature: Ingress Gateway, which uses Envoy proxy as the gateway (as opposed to nginx).