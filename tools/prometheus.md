# Prometheus

**Prometheus** is an open-source systems monitoring and alerting toolkit.

Prometheus collects and stores its *metrics* as time series data, i.e. metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called *labels*.

Prometheus collects metrics from *targets* by scraping metrics HTTP endpoints. Since Prometheus exposes data in the same manner about itself, it can also scrape and monitor its own health.

- [Prometheus](#prometheus)
  - [Data Model](#data-model)
  - [Metric types](#metric-types)
    - [Counter](#counter)
    - [Gauge](#gauge)
    - [Histogram](#histogram)
    - [Summary](#summary)
  - [Jobs and instances](#jobs-and-instances)
  - [Exporters and collectors](#exporters-and-collectors)
  - [PromQL](#promql)
  - [Run as a Docker container](#run-as-a-docker-container)

## Data Model

A **metric** is a quantitative measure of some software characteristic.

Prometheus fundamentally stores all data as time series: streams of timestamped values, called *samples*, belonging to the same metric and the same set of labeled dimensions.

Every time series is uniquely identified by its **metric name** and optional key-value pairs called **labels**.

The metric name specifies the general feature of a system that is measured, e.g. `http_requests_total` — the total number of HTTP requests received. It may contain ASCII letters and digits, as well as underscores and colons. It must match the regex `[a-zA-Z_:][a-zA-Z0-9_:]*`.

> The colons are reserved for user defined recording rules. They should not be used by exporters or direct instrumentation.

Labels enable Prometheus's dimensional data model: any given combination of labels for the same metric name identifies a particular dimensional instantiation of that metric. For example: all HTTP requests that used the method `POST` to the `/api/tracks` handler. The query language allows filtering and aggregation based on these dimensions. Changing any label value, including adding or removing a label, will create a new time series.

Label names may contain ASCII letters, numbers, as well as underscores. They must match the regex `[a-zA-Z_][a-zA-Z0-9_]*`. Label names beginning with `__` are reserved for internal use.

Label values may contain any Unicode characters.

A label with an empty label value is considered equivalent to a label that does not exist.

See also the [↑ best practices for naming metrics and labels](https://prometheus.io/docs/practices/naming/).

> CAUTION: Remember that every unique combination of key-value label pairs represents a new time series, which can dramatically increase the amount of data stored. Do not use labels to store dimensions with high cardinality (many different label values), such as user IDs, email addresses, or other unbounded sets of values.

A **sample** is a single value at a point in time in a time series. Samples form the actual time series data. Each sample consists of:

- a float64 value
- a millisecond-precision timestamp

Given a metric name and a set of labels, time series are frequently identified using this notation:

```text
metric_name{label_name=label_value, ...}
```

For example, a time series with the metric name `api_http_requests_total` and the labels `method="POST"` and `handler="/messages"` could be written like this:

```text
api_http_requests_total{method="POST", handler="/messages"}
```

## Metric types

The Prometheus client libraries offer four core metric types. These are currently only differentiated in the client libraries, to enable APIs tailored to the usage of the specific types, and in the wire protocol. The Prometheus server does not yet make use of the type information and flattens all data into untyped time series. This may change in the future.

### Counter

A **counter** is a cumulative metric that represents a single monotonically increasing counter whose value can only increase or be reset to zero on restart.

Do not use a counter to expose a value that can decrease. For example, do not use a counter for the number of currently running processes; instead use a *gauge*.

### Gauge

A **gauge** is a metric that represents a single numerical value that can arbitrarily go up and down.

Gauges are typically used for measured values like temperatures or current memory usage, but also "counts" that can go up and down, like the number of concurrent requests.

> To pick between counter and gauge, there is a simple rule of thumb: if the value can go down, it is a gauge

### Histogram

A **histogram** samples observations, usually things like request durations or response sizes, and counts them in configurable buckets. It also provides a sum of all observed values.

A histogram with a base metric name of `basename` exposes multiple time series during a scrape:

- cumulative counters for the observation buckets, exposed as `basename_bucket{le="upper inclusive bound"}`
- the **total sum** of all observed values, exposed as `basename_sum`
- the **count** of events that have been observed, exposed as `basename_count`, identical to `basename_bucket{le="+Inf"}` above

Use the `histogram_quantile()` function to calculate *quantiles* from histograms or even aggregations of histograms. A histogram is also suitable to calculate an Apdex score.

> A **quantile** defines a particular part of a data set, i.e. a quantile determines how many values in a distribution are above or below a certain limit. Special quantiles are the quartile (quarter), the quintile (fifth) and percentiles (hundredth).

When operating on buckets, remember that the histogram is cumulative.

### Summary

A **summary**, which is similar to a histogram, samples observations, usually things like request durations and response sizes. While it also provides a total count of observations and a sum of all observed values, it calculates configurable quantiles over a sliding time window.

A summary with a base metric name of `basename` exposes multiple time series during a scrape:

- streaming **φ-quantiles** (0 ≤ φ ≤ 1) of observed events, exposed as `basename{quantile="φ"}`
- the **total sum** of all observed values, exposed as `basename_sum`
- the **count** of events that have been observed, exposed as `basename_count`

See [↑ histograms and summaries](https://prometheus.io/docs/practices/histograms/) for detailed explanations of φ-quantiles, summary usage, and differences to histograms.

## Jobs and instances

An **instance** is an endpoint you can scrape, usually corresponding to a single process.

A **job** is a collection of instances with the same purpose, a process replicated for scalability or reliability for example.

For example, an API server job with four replicated instances:

- job: `api-server`
  - instance 1: `1.2.3.4:5670`
  - instance 2: `1.2.3.4:5671`
  - instance 4: `5.6.7.8:5671`
  - instance 3: `5.6.7.8:5670`

When Prometheus scrapes a target, it attaches some labels automatically to the scraped time series which serve to identify the scraped target:

- `job`: The configured job name that the target belongs to.
- `instance`: The `host:port` part of the target's URL that was scraped.

For each instance scrape, Prometheus stores a sample in the following time series:

- `up{job="job name", instance="instance id"}`: `1` if the instance is healthy, i.e. reachable, or `0` if the scrape failed.
- `scrape_duration_seconds{job="job name", instance="instance id"}`: duration of the scrape.
- `scrape_samples_post_metric_relabeling{job="job name", instance="instance id"}`: the number of samples remaining after metric relabeling was applied.
- `scrape_samples_scraped{job="job name", instance="instance id"}`: the number of samples the target exposed.
- `scrape_series_added{job="job name", instance="instance id"}`: the approximate number of new series in this scrape.

The `up` time series is useful for instance availability monitoring.

## Exporters and collectors

An **exporter** is a binary running alongside the application you want to obtain metrics from. The exporter exposes Prometheus metrics, commonly by converting metrics that are exposed in a non-Prometheus format into a format that Prometheus supports.

A **collector** is a part of an exporter that represents a set of metrics. It may be a single metric if it is part of direct instrumentation, or many metrics if it is pulling metrics from another system.

## PromQL

**PromQL** is the Prometheus Query Language. It allows for a wide range of operations including aggregation, slicing and dicing, prediction and joins.

## Run as a Docker container

Create `prometheus.yaml` file and put it under `/Users/aleksei/prometheus` folder:

```yaml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]
```

Modify `prometheus.yaml` file in Rider by adding this lines:

```yaml
  - job_name: 'dictionary.words'
    static_configs: 
      - targets: ['host.docker.internal:5000']
    metrics_path: /metrics-text
```

Example of [↑ prometheus.yml](https://github.com/prometheus/prometheus/blob/main/documentation/examples/prometheus.yml) file:

Run Docker container:

```bash
docker run \
    -d \
    -p 9090:9090 \
    -v /Users/aleksei/prometheus:/etc/prometheus \
    --name prometheus \
    prom/prometheus
```
