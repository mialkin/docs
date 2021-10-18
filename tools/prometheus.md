# Prometheus

Prometheus is an open-source systems monitoring and alerting toolkitю

Prometheus collects and stores its metrics as time series data, i.e. metrics information is stored with the timestamp at which it was recorded, alongside optional key-value pairs called labels.

- [Prometheus](#prometheus)
  - [Data Model](#data-model)
  - [Metric Types](#metric-types)

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

**Samples** form the actual time series data. Each sample consists of:

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
