# Prometheus

Prometheus is an open-source systems monitoring and alerting toolkitю

Prometheus collects and stores its metrics as time series data, i.e. metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called labels.

- [Prometheus](#prometheus)
  - [Data Model](#data-model)
  - [Metric Types](#metric-types)
  - [Jobs And Instances](#jobs-and-instances)
  - [Exporters And Collectors](#exporters-and-collectors)
  - [PromQL](#promql)

## Data Model

Prometheus fundamentally stores all data as time series: streams of timestamped values belonging to the same metric and the same set of labeled dimensions.

Every time series is uniquely identified by its **metric name** and optional key-value pairs called **labels**.

The metric name specifies the general feature of a system that is measured (e.g. `http_requests_total` - the total number of HTTP requests received). It may contain ASCII letters and digits, as well as underscores and colons. It must match the regex `[a-zA-Z_:][a-zA-Z0-9_:]*`.

Note: The colons are reserved for user defined recording rules. They should not be used by exporters or direct instrumentation.

Labels enable Prometheus's dimensional data model: any given combination of labels for the same metric name identifies a particular dimensional instantiation of that metric (for example: all HTTP requests that used the method `POST` to the `/api/tracks` handler). The query language allows filtering and aggregation based on these dimensions. Changing any label value, including adding or removing a label, will create a new time series.

Label names may contain ASCII letters, numbers, as well as underscores. They must match the regex `[a-zA-Z_][a-zA-Z0-9_]*`. Label names beginning with `__` are reserved for internal use.

Label values may contain any Unicode characters.

A label with an empty label value is considered equivalent to a label that does not exist.

See also the [↑ best practices for naming metrics and labels](https://prometheus.io/docs/practices/naming/).

A **sample** is a single value at a point in time in a time series. Samples form the actual time series data. Each sample consists of:

- a float64 value
- a millisecond-precision timestamp

Given a metric name and a set of labels, time series are frequently identified using this notation:

```text
<metric name>{<label name>=<label value>, ...}
```

For example, a time series with the metric name `api_http_requests_total` and the labels `method="POST"` and `handler="/messages"` could be written like this:

```text
api_http_requests_total{method="POST", handler="/messages"}
```

## Metric Types

The Prometheus client libraries offer four core metric types. These are currently only differentiated in the client libraries (to enable APIs tailored to the usage of the specific types) and in the wire protocol. The Prometheus server does not yet make use of the type information and flattens all data into untyped time series. This may change in the future.

A **counter** is a cumulative metric that represents a single monotonically increasing counter whose value can only increase or be reset to zero on restart.

Do not use a counter to expose a value that can decrease. For example, do not use a counter for the number of currently running processes; instead use a gauge.

A **gauge** is a metric that represents a single numerical value that can arbitrarily go up and down.

Gauges are typically used for measured values like temperatures or current memory usage, but also "counts" that can go up and down, like the number of concurrent requests.

> To pick between counter and gauge, there is a simple rule of thumb: if the value can go down, it is a gauge.

A **histogram** samples observations (usually things like request durations or response sizes) and counts them in configurable buckets. It also provides a sum of all observed values.

A histogram with a base metric name of  `basename` exposes multiple time series during a scrape:

- cumulative counters for the observation buckets, exposed as `basename_bucket{le="upper inclusive bound>"}`
- the **total sum** of all observed values, exposed as `basename_sum`
- the **count** of events that have been observed, exposed as `basename_count` (identical to `basename_bucket{le="+Inf"}` above)

Use the `histogram_quantile()` function to calculate quantiles from histograms or even aggregations of histograms. A histogram is also suitable to calculate an Apdex score. When operating on buckets, remember that the histogram is cumulative.

A **summary**, which is similar to a histogram, samples observations (usually things like request durations and response sizes). While it also provides a total count of observations and a sum of all observed values, it calculates configurable quantiles over a sliding time window.

A summary with a base metric name of`basename` exposes multiple time series during a scrape:

streaming **φ-quantiles** (0 ≤ φ ≤ 1) of observed events, exposed as `basename{quantile="<φ>"}`
the **total sum** of all observed values, exposed as `basename_sum`
the count of events that have been observed, exposed as `basename_count`

See [↑ histograms and summaries](https://prometheus.io/docs/practices/histograms/) for detailed explanations of φ-quantiles, summary usage, and differences to histograms.

## Jobs And Instances

In Prometheus terms, an endpoint you can scrape is called an **instance**, usually corresponding to a single process. A collection of instances with the same purpose, a process replicated for scalability or reliability for example, is called a **job**.

For example, an API server job with four replicated instances:

- job: `api-server`
  - instance 1: `1.2.3.4:5670`
  - instance 2: `1.2.3.4:5671`
  - instance 4: `5.6.7.8:5671`
  - instance 3: `5.6.7.8:5670`

When Prometheus scrapes a target, it attaches some labels automatically to the scraped time series which serve to identify the scraped target:

- `job`: The configured job name that the target belongs to.
- `instance`: The `<host>:<port>` part of the target's URL that was scraped.

For each instance scrape, Prometheus stores a sample in the following time series:

- `up{job="<job-name>", instance="<instance-id>"}`: `1` if the instance is healthy, i.e. reachable, or `0` if the scrape failed.
- `scrape_duration_seconds{job="<job-name>", instance="<instance-id>"}`: duration of the scrape.
- `scrape_samples_post_metric_relabeling{job="<job-name>", instance="<instance-id>"}`: the number of samples remaining after metric relabeling was applied.
- `scrape_samples_scraped{job="<job-name>", instance="<instance-id>"}`: the number of samples the target exposed.
- `scrape_series_added{job="<job-name>", instance="<instance-id>"}`: the approximate number of new series in this scrape.

The `up` time series is useful for instance availability monitoring.

## Exporters And Collectors

An **exporter** is a binary running alongside the application you want to obtain metrics from. The exporter exposes Prometheus metrics, commonly by converting metrics that are exposed in a non-Prometheus format into a format that Prometheus supports.

A **collector** is a part of an exporter that represents a set of metrics. It may be a single metric if it is part of direct instrumentation, or many metrics if it is pulling metrics from another system.

## PromQL

**PromQL** is the Prometheus Query Language. It allows for a wide range of operations including aggregation, slicing and dicing, prediction and joins.
