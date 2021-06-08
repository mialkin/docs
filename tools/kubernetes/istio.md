# Istio

Label namespaces that will have automatic sidecar injection:

```bash
kubectl label namespace NAMESPACE_NAME istio-injection=enabled
```

Determine your external IP:

```bash
kubectl get svc istio-ingressgateway -n istio-system
```

List Istio ingress gateways:

```bash
kubectl get gateway --all-namespaces
```

Access the Kiali dashboard:

```bash
istioctl dashboard kiali
```
