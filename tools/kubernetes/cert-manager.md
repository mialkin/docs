# cert-manager

[↑ Installing with Helm](https://cert-manager.io/next-docs/installation/kubernetes/#installing-with-helm):

```bash
kubectl create namespace cert-manager
helm repo add jetstack https://charts.jetstack.io
```

```bash
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.3.1 \
  --set installCRDs=true
```
