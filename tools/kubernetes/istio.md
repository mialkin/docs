# Istio

## Table of contents

- [Istio](#istio)
  - [Table of contents](#table-of-contents)
    - [Kubernetes Ingress vs Istio Gateway](#kubernetes-ingress-vs-istio-gateway)

### Kubernetes Ingress vs Istio Gateway

Any application running at production scale should have an "ingress" to expose itself to the outside world. While Kubernetes provides the `Ingress` resource for this purpose, its feature set is limited depending on the kind of ingress controller (usually Nginx) being used. Alternatively, you can leverage Istio and take advantage of its more feature-rich `Gateway` resource, even if your application Pods themselves are not running purely Kubernetes. We can do so by incrementally adopting Istio’s feature: Ingress Gateway, which uses Envoy proxy as the gateway (as opposed to nginx).