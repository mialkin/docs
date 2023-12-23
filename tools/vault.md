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
  - [Vault Agent with Kubernetes](#vault-agent-with-kubernetes)
    - [Start a Vault server](#start-a-vault-server)
    - [Start minikube](#start-minikube)
    - [Next](#next)

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

## Vault Agent with Kubernetes

[↑ Vault Agent with Kubernetes](https://developer.hashicorp.com/vault/tutorials/vault-agent/agent-kubernetes).

### Start a Vault server

1\. Start a Vault dev server which listens for requests locally at `0.0.0.0:8200` with `root` as the root token ID:

```bash
vault server \
-dev \
-dev-root-token-id root \
-dev-listen-address 0.0.0.0:8200
```

Setting the `-dev-listen-address` to `0.0.0.0:8200` overrides the default address of a Vault dev server `127.0.0.1:8200` and enables Vault to be addressable by the Kubernetes cluster and its pods because it binds to a shared network.

2\. Export an environment variable for the vault CLI to address the Vault server.

```bash
export VAULT_ADDR=http://0.0.0.0:8200
```

### Start minikube

```bash
#cd ~/Downloads
#curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
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
