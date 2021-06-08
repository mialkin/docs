# Kiali

## Installation

Kali relies on Prometheus.

[↑ Install](https://kiali.io/documentation/latest/quick-start) Kiali using Helm Chart:

```bash
helm install \
  --namespace istio-system \
  --set auth.strategy="anonymous" \
  --repo https://kiali.org/helm-charts \
  kiali-server \
  kiali-server
```

## Access to the UI

```bash
istioctl dashboard kiali
```
