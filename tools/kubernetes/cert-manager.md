# cert-manager

[↑ cert-manager](https://cert-manager.io/docs) is a powerful and extensible X.509 certificate controller for Kubernetes and OpenShift workloads.

cert-manager adds certificates and certificate issuers as resource types in Kubernetes clusters, and simplifies the process of obtaining, renewing and using those certificates.

It can issue certificates from a variety of supported sources, including [↑ Let's Encrypt](https://letsencrypt.org) and [↑ HashiCorp Vault](https://www.vaultproject.io).

It will ensure certificates are valid and up to date, and attempt to renew certificates at a configured time before expiry.

## Installation

### x86

[↑ Installing with Helm](https://cert-manager.io/docs/installation/helm):

Install Helm:

```bash
brew install helm
```

Create `cert-manager` namespace and add jetstack's repository:

```bash
helm repo list
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

Install cert-manager:

```bash
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.9.1 \
  --set installCRDs=true
```

### ARM64

```bash
curl -sL \
https://github.com/jetstack/cert-manager/releases/download/v1.5.3/cert-manager.yaml |\
sed -r 's/(image:.*):(v.*)$/\1-arm64:\2/g' > cert-manager-arm.yaml

kubectl create namespace cert-manager
kubectl create -f cert-manager-arm.yaml
```
