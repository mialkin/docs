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

## ARM64

```bash
curl -sL \
https://github.com/jetstack/cert-manager/releases/download/v1.5.3/cert-manager.yaml |\
sed -r 's/(image:.*):(v.*)$/\1-arm64:\2/g' > cert-manager-arm.yaml

kubectl create namespace cert-manager
kubectl create -f cert-manager-arm.yaml
```
