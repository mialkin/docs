# Vault

[↑ Vault](https://www.vaultproject.io) is a key management service provider.

## Run Vault in Docker locally

```bash
docker run --cap-add=IPC_LOCK -e 'VAULT_LOCAL_CONFIG={"storage": {"file": {"path": "/vault/file"}}, "listener": [{"tcp": { "address": "0.0.0.0:8200", "tls_disable": true}}], "default_lease_ttl": "168h", "max_lease_ttl": "720h", "ui": true}' \
-p 8200:8200 hashicorp/vault server
```

[↑ hashicorp/vault](https://hub.docker.com/r/hashicorp/vault).

## Run Vault on Kubernetes

The main Vault use case with relevance to Kubernetes is its secret management capabilities. Vault can store API keys, database credentials, environmental variables and more.

[↑ Run Vault on Kubernetes](https://developer.hashicorp.com/vault/docs/platform/k8s/helm/run).

### Helm chart

The Vault Helm chart is the recommended way to install and configure Vault on Kubernetes.

By default, the chart runs in standalone mode. This mode uses a single Vault server with a file storage backend. This is a less secure and less resilient installation that is NOT appropriate for a production setup.
