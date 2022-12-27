# PromQL

**PromQL**, stands for [↑ Prometheus query language](https://prometheus.io/docs/prometheus/latest/querying/basics), is a functional query language that lets the user select and aggregate time series data in real time. It allows for a wide range of operations including aggregation, slicing and dicing, prediction and joins.

The result of a PromQL expression can either be shown as a graph, viewed as tabular data in Prometheus's [↑ expression browser](http://localhost:9090/graph), or consumed by external systems via the [↑ HTTP API](https://prometheus.io/docs/prometheus/latest/querying/api).

See also [↑ MetricsQL](https://docs.victoriametrics.com/MetricsQL.html).

## Table of contents

- [PromQL](#promql)
  - [Table of contents](#table-of-contents)
  - [Expression language data types](#expression-language-data-types)
    - [Instant vector](#instant-vector)
    - [Range vector](#range-vector)
    - [Scalar](#scalar)
    - [String](#string)
  - [Functions](#functions)
    - [`rate(v range-vector)`](#ratev-range-vector)

## Expression language data types

In Prometheus's expression language, an expression or sub-expression can evaluate to one of four types:

- Instant vector
- Range vector
- Scalar
- String

Depending on the use-case (e.g. when graphing vs. displaying the output of an expression), only some of these types are legal as the result from a user-specified expression. 

### Instant vector

An **instant vector** is a set of time series containing a single sample for each time series, all sharing the same timestamp.

### Range vector

A **range vector** is a set of time series containing a range of data points over time for each time series.

### Scalar

A **scalar** is a simple numeric floating point value.

### String

A **string** is a simple string value; currently unused.

## Functions

[↑ Functions](https://prometheus.io/docs/prometheus/latest/querying/functions).

### `rate(v range-vector)`

The `rate(m[d])` function calculates the increase of a counter metric `m` over the given lookbehind window `d` in square brackets and then divides the increase by `d`. The calculation is performed independently per each matching time series `m`. For example, suppose there are `http_requests_total` metrics with `url` label:

```text
http_requests_total{url="/foo"}
http_requests_total{url="/bar"}
```

If they have the following values at time `t0`:

```text
http_requests_total{url="/foo"} 123
http_requests_total{url="/bar"} 456
```

and the following values at time `t0 + 5` minutes:

```text
http_requests_total{url="/foo"} 345
http_requests_total{url="/bar"} 789
```

Then `rate(http_requests_total[5m])` at time `t0 + 5` minutes is calculated in the following way:

1\. Calculate increase for these metrics between `t0` and `t0 + 5` minutes:

```text
increase(http_requests_total{url="/foo"}[5m]) = 345 - 123 = 222
increase(http_requests_total{url="/bar"}[5m]) = 789 - 456 = 333
```

2\. Divide the calculated increase by `5 minutes` expressed in seconds (`5 * 60s = 300s`):

```text
rate(http_requests_total{url="/foo"}[5m]) = 222 / 300 = 0.74
rate(http_requests_total{url="/bar"}[5m]) = 333 / 300 = 1.11
```

The end result of `rate(http_requests_total[5m])` is a per-second average rps for the last `5` minutes, which is calculated individually per each time series with `http_requests_total` name.
