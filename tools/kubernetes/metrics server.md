# Metrics server

Through the Kubernetes' Metrics API, you can get the amount of resource currently used by a given node or a given pod. This API doesn't store the metric values, so it's not possible, for example, to get the amount of resources used by a given node 10 minutes ago.

## Installation

Kubernetes Metrics Server [↑ GitHub repository](https://github.com/kubernetes-sigs/metrics-server).

Don't forget to modify `yaml`-file in case of [↑ discovery problems](https://github.com/kubernetes-sigs/metrics-server/issues/131):

```yaml
args:
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP
```
