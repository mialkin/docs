# Vault

[↑ Vault](https://www.vaultproject.io) is a key management service provider.

## Table of contents

- [Vault](#vault)
  - [Table of contents](#table-of-contents)
  - [Components](#components)
    - [Path](#path)
    - [Secret engine](#secret-engine)
    - [Vault Agent](#vault-agent)
    - [Seal/unseal](#sealunseal)
      - [Shamir seals](#shamir-seals)
    - [Sealing](#sealing)
  - [Kubernetes](#kubernetes)
  - [Run](#run)
    - [Docker locally](#docker-locally)
    - [On Kubernetes](#on-kubernetes)
    - [Homebrew](#homebrew)
  - [Injecting secrets into Kubernetes pods via Vault Agent containers](#injecting-secrets-into-kubernetes-pods-via-vault-agent-containers)
    - [Clone GitHub repository](#clone-github-repository)
    - [Start minikube](#start-minikube)
    - [Next](#next)
    - [Install the Vault Helm chart](#install-the-vault-helm-chart)
    - [Set a secret in Vault](#set-a-secret-in-vault)
    - [Configure Kubernetes authentication](#configure-kubernetes-authentication)
    - [Define a Kubernetes service account](#define-a-kubernetes-service-account)
    - [Launch an application](#launch-an-application)
    - [Inject secrets into the pod](#inject-secrets-into-the-pod)

## Components

### Path

A **path** is the parameter used for `vault read`, `vault write`, etc. An example path is `secret/foo`, or `aws/config/root`. The paths available depend on the [secrets engines](#secret-engine) in use.

In Vault, everything is path based. This means that every operation that is performed in Vault is done through a path. The path is used to determine the location of the operation, as well as the permissions that are required to execute the operation.

[↑ path-help](https://developer.hashicorp.com/vault/docs/commands/path-help).

### Secret engine

A **secret engine** is a component that stores, generates, or encrypts data.

Secrets engines are enabled at a [path](#path) in Vault. When a request comes to Vault, the router automatically routes anything with the route prefix to the secrets engine. In this way, each secrets engine defines its own paths and properties. To the user, secrets engines behave similar to a virtual filesystem, supporting operations like read, write, and delete.

[↑ Secret engines](https://developer.hashicorp.com/vault/docs/secrets).

[↑ Cubbyhole secrets engine](https://developer.hashicorp.com/vault/docs/secrets/cubbyhole).

### Vault Agent

A **Vault Agent** is a client-side daemon that makes requests to Vault on behalf of the client application. This includes the authentication to Vault.

Vault clients like human users, applications, etc., must authenticate with Vault and get a client token to make API requests. Because tokens have time-to-live, TTL, the clients must renew the token's TTL or re-authenticate to Vault based on its TTL. Vault Agent authenticates with Vault and manage the token's lifecycle so that the client application doesn't have to.

[↑ Vault Agent quick start](https://developer.hashicorp.com/vault/tutorials/vault-agent/agent-quick-start).

### Seal/unseal

When a Vault server is started, it starts in a *sealed* state. In this state, Vault is configured to know where and how to access the physical storage, but doesn't know how to decrypt any of it.

*Unsealing* is the process of obtaining the plaintext *root key* necessary to read the decryption key to decrypt the data, allowing access to the Vault. Prior to unsealing, almost no operations are possible with Vault.

The data stored by Vault is encrypted. Vault needs the *encryption key* in order to decrypt the data. The encryption key is also stored with the data in the *keyring*, but encrypted with another encryption key known as the *root key*.

Therefore, to decrypt the data, Vault must decrypt the encryption key which requires the root key. Unsealing is the process of getting access to this root key. The root key is stored alongside all other Vault data, but is encrypted by yet another mechanism: the *unseal key*.

To recap: most Vault data is encrypted using the encryption key in the keyring; the keyring is encrypted by the root key; and the root key is encrypted by the unseal key.

[↑ Seal/Unseal](https://developer.hashicorp.com/vault/docs/concepts/seal).

#### Shamir seals

The default Vault config uses a Shamir seal. Instead of distributing the unseal key as a single key to an operator, Vault uses an algorithm known as [↑ Shamir's secret sharing](https://en.wikipedia.org/wiki/Shamir%27s_secret_sharing) to split the key into shares. A certain threshold of shares is required to reconstruct the unseal key, which is then used to decrypt the root key.

This is the *unseal process*: the shares are added one at a time, in any order, until enough shares are present to reconstruct the key and decrypt the root key.

### Sealing

There is also an API to seal the Vault. This will throw away the root key in memory and require another unseal process to restore it. Sealing only requires a single operator with root privileges.

This way, if there is a detected intrusion, the Vault data can be locked quickly to try to minimize damages. It can't be accessed again without access to the root key shares.

## Kubernetes

Vault provides [↑ Kubernetes auth method](https://developer.hashicorp.com/vault/docs/auth/kubernetes) to authenticate the clients using a [↑ Kubernetes Service Account](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin) Token.

## Run

### Docker locally

```bash
docker run --cap-add=IPC_LOCK -e 'VAULT_LOCAL_CONFIG={"storage": {"file": {"path": "/vault/file"}}, "listener": [{"tcp": { "address": "0.0.0.0:8200", "tls_disable": true}}], "default_lease_ttl": "168h", "max_lease_ttl": "720h", "ui": true}' \
-p 8200:8200 hashicorp/vault server
```

[↑ hashicorp/vault](https://hub.docker.com/r/hashicorp/vault).

### On Kubernetes

The main Vault use case with relevance to Kubernetes is its secret management capabilities. Vault can store API keys, database credentials, environmental variables and more.

[↑ Vault on Kubernetes deployment guide](https://developer.hashicorp.com/vault/tutorials/kubernetes/kubernetes-raft-deployment-guide).

[↑ Run Vault on Kubernetes](https://developer.hashicorp.com/vault/docs/platform/k8s/helm/run).

The Vault Helm chart is the recommended way to install and configure Vault on Kubernetes.

By default, the chart runs in standalone mode. This mode uses a single Vault server with a file storage backend. This is a less secure and less resilient installation that is NOT appropriate for a production setup.

### Homebrew

Install the HashiCorp tap, a repository of all HashiCorp Homebrew packages:

```bash
brew tap hashicorp/tap
```

Install Vault:

```bash
brew install hashicorp/tap/vault
```

To start Vault now and restart at login:

```bash
brew services start hashicorp/tap/vault
```
  
Or, if you don't want/need a background service you can just run:

```bash
vault server -dev
```

## Injecting secrets into Kubernetes pods via Vault Agent containers

[↑ Injecting secrets into Kubernetes pods via Vault Agent containers](https://developer.hashicorp.com/vault/tutorials/kubernetes/kubernetes-sidecar).

### Clone GitHub repository

```bash
git clone https://github.com/hashicorp-education/learn-vault-kubernetes-sidecar.git
cd learn-vault-kubernetes-sidecar
```

### Start minikube

```bash
cd ~/Downloads
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
minikube config set driver docker
minikube start
#minikube start --driver=docker
#minikube delete
```

### Next

```bash
git clone https://github.com/hashicorp/learn-vault-agent.git
cd learn-vault-agent/vault-agent-k8s-demo
kubectl apply --filename vault-auth-service-account.yaml
```

### Install the Vault Helm chart

```bash
helm repo add hashicorp https://helm.releases.hashicorp.com
helm repo update
helm install vault hashicorp/vault --set "server.dev.enabled=true"
```

### Set a secret in Vault

```bash
# The `vault-0` pod runs a Vault server in development mode.
# The `vault-agent-injector` pod performs the injection based on the annotations
# present or patched on a deployment.
kubectl exec -it vault-0 -- /bin/sh
vault secrets enable -path=internal kv-v2
vault kv put internal/database/config username="db-readonly-username" password="db-secret-password"
vault kv get internal/database/config
exit
```

### Configure Kubernetes authentication

```bash
kubectl exec -it vault-0 -- /bin/sh

# Enable the Kubernetes authentication method
vault auth enable kubernetes

# Configure the Kubernetes authentication method to use the location of the Kubernetes API
vault write auth/kubernetes/config \
      kubernetes_host="https://$KUBERNETES_PORT_443_TCP_ADDR:443"
```

```bash
# Write out the policy named `internal-app` that enables the read capability for secrets 
# at path `internal/data/database/config`
vault policy write internal-app - <<EOF
path "*" {
   capabilities = ["read"]
}
EOF
# path "internal/data/database/config"
```

```bash
# Create a Kubernetes authentication role named `internal-app`
vault write auth/kubernetes/role/internal-app \
      bound_service_account_names=internal-app \
      bound_service_account_namespaces=default \
      policies=internal-app \
      ttl=24h

exit
```

The role connects the Kubernetes service account, `internal-app`, and namespace, `default`, with the Vault policy, `internal-app`. The tokens returned after authentication are valid for 24 hours.

### Define a Kubernetes service account

A service account provides an identity for processes that run in a Pod. With this identity we will be able to run the application within the cluster.

```bash
# Get all the service accounts in the default namespace
kubectl get serviceaccounts

# Create a Kubernetes service account named `internal-app` in the default namespace
kubectl create sa internal-app
```

The name of the service account here aligns with the name assigned to the `bound_service_account_names` field when the `internal-app` role was created.

### Launch an application

```bash
# Apply the deployment defined in `deployment-orgchart.yaml` file
kubectl apply --filename deployment-orgchart.yaml
kubectl get pods
```

The name of this deployment is `orgchart`. The `spec.template.spec.serviceAccountName` defines the service account `internal-app` to run this container.

```bash
# Verify that no secrets are written to the `orgchart` container in the `orgchart` pod
kubectl exec \
$(kubectl get pod -l app=orgchart -o jsonpath="{.items[0].metadata.name}") \
--container orgchart -- ls /vault/secrets
```

The output displays that there is no such file or directory named `/vault/secrets`:

```console
ls: /vault/secrets: No such file or directory
command terminated with exit code 1
```

### Inject secrets into the pod

The deployment is running the pod with the `internal-app` Kubernetes service account in the default namespace. The Vault Agent Injector only modifies a deployment if it contains a specific set of annotations. An existing deployment may have its definition patched to include the necessary annotations.

```bash
# Patch the orgchart deployment defined in patch-inject-secrets.yaml
kubectl patch deployment orgchart --patch "$(cat patch-inject-secrets.yaml)"

# Display the secret written to the `orgchart` container in the `orgchart` pod
kubectl exec \
      $(kubectl get pod -l app=orgchart -o jsonpath="{.items[0].metadata.name}") \
      -c orgchart -- cat /vault/secrets/database-config.txt
```
