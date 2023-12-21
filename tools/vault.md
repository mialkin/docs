# Vault

[↑ Vault](https://www.vaultproject.io) is a key management service provider.

## Table of contents

- [Vault](#vault)
  - [Table of contents](#table-of-contents)
  - [Components](#components)
    - [Path](#path)
    - [Secret engine](#secret-engine)
    - [Vault Agent](#vault-agent)
  - [Kubernetes](#kubernetes)
  - [Run](#run)
    - [Docker locally](#docker-locally)
    - [On Kubernetes](#on-kubernetes)

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
