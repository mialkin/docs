# Metrics server

Kubernetes Metrics Server [↑ GitHub repository](https://github.com/kubernetes-sigs/metrics-server).

Don't forget to modify `yaml`-file in case of [↑ discovery problems](https://github.com/kubernetes-sigs/metrics-server/issues/131):

```yaml
args:
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP
```
