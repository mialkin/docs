# Prometheus

**Prometheus** is an open source monitoring system and time series database. You can use Prometheus with Istio to record metrics that track the health of Istio and of applications within the service mesh. You can visualize metrics using tools like **Grafana** and **Kiali**.

## Installation

You can [↑ install](https://bitnami.com/stack/prometheus-operator/helm) Prometheus as a Helm Chart:

```bash
helm install my-release bitnami/kube-prometheus
```

Watch the Prometheus Operator Deployment status using the command:

    kubectl get deploy -w --namespace default -l app.kubernetes.io/name=kube-prometheus-operator,app.kubernetes.io/instance=my-prometheus

Watch the Prometheus StatefulSet status using the command:

    kubectl get sts -w --namespace default -l app.kubernetes.io/name=kube-prometheus-prometheus,app.kubernetes.io/instance=my-prometheus

Prometheus can be accessed via port "9090" on the following DNS name from within your cluster:

    my-prometheus-kube-prometh-prometheus.default.svc.cluster.local

To access Prometheus from outside the cluster execute the following commands:

    echo "Prometheus URL: http://127.0.0.1:9090/"
    kubectl port-forward --namespace default svc/my-prometheus-kube-prometh-prometheus 9090:9090

Watch the Alertmanager StatefulSet status using the command:

    kubectl get sts -w --namespace default -l app.kubernetes.io/name=kube-prometheus-alertmanager,app.kubernetes.io/instance=my-prometheus

Alertmanager can be accessed via port "9093" on the following DNS name from within your cluster:

    my-prometheus-kube-prometh-alertmanager.default.svc.cluster.local

To access Alertmanager from outside the cluster execute the following commands:

    echo "Alertmanager URL: http://127.0.0.1:9093/"
    kubectl port-forward --namespace default svc/my-prometheus-kube-prometh-alertmanager 9093:9093