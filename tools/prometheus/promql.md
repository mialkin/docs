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
      - [Time durations](#time-durations)
      - [Offset modifier](#offset-modifier)
      - [`@` modifier](#-modifier)
    - [Scalar](#scalar)
    - [String](#string)
  - [Functions](#functions)
    - [`increase(v range-vector)`](#increasev-range-vector)
    - [`rate(v range-vector)`](#ratev-range-vector)
    - [`resets(v range-vector)`](#resetsv-range-vector)

## Expression language data types

In Prometheus's expression language, an expression or sub-expression can evaluate to one of four types:

- Instant vector
- Range vector
- Scalar
- String

Depending on the use-case (e.g. when graphing vs displaying the output of an expression), only some of these types are legal as the result from a user-specified expression.

### Instant vector

An **instant vector** is a set of time series containing a single sample for each time series, all sharing the same timestamp.

Instant vector selectors allow the selection of a set of time series and a single sample value for each at a given timestamp i.e. *instant*: in the simplest form, only a metric name is specified. This results in an instant vector containing elements for all time series that have this metric name.

This example selects all time series that have the `http_requests_total` metric name:

```text
http_requests_total
```

It is possible to filter these time series further by appending a comma separated list of label matchers in curly braces (`{}`).

This example selects only those time series with the `http_requests_total` metric name that also have the `job` label set to `prometheus` and their group label set to `canary`:

```text
http_requests_total{job="prometheus",group="canary"}
```

It is also possible to negatively match a label value, or to match label values against regular expressions. The following label matching operators exist:

- `=:` select labels that are exactly equal to the provided string
- `!=:` select labels that are not equal to the provided string
- `=~:` select labels that regex-match the provided string
- `!~:` select labels that do not regex-match the provided string

Regex matches are fully anchored. A match of `env=~"foo"` is treated as `env=~"^foo$"`.

For example, this selects all `http_requests_total` time series for `staging`, `testing`, and `development` environments and HTTP methods other than `GET`:

```text
http_requests_total{environment=~"staging|testing|development",method!="GET"}
```

Label matchers that match empty label values also select all time series that do not have the specific label set at all. It is possible to have multiple matchers for the same label name.

Vector selectors must either specify a name or at least one label matcher that does not match the empty string. The following expression is illegal:

```text
{job=~".*"} # Bad!
```

In contrast, these expressions are valid as they both have a selector that does not match empty label values:

```text
{job=~".+"}              # Good!
{job=~".*",method="get"} # Good!
```

Label matchers can also be applied to metric names by matching against the internal `__name__` label. For example, the expression `http_requests_total` is equivalent to `{__name__="http_requests_total"}`. Matchers other than `=` (`!=`, `=~`, `!~`) may also be used. The following expression selects all metrics that have a name starting with `job:`:

```text
{__name__=~"job:.*"}
```

### Range vector

[↑ Understanding Prometheus Range Vectors](https://satyanash.net/software/2021/01/04/understanding-prometheus-range-vectors.html)

A **range vector** is a set of time series containing a range of data points over time for each time series.

Range vector literals work like instant vector literals, except that they select a range of samples back from the current instant. Syntactically, a time duration is appended in square brackets (`[]`) at the end of a vector selector to specify how far back in time values should be fetched for each resulting range vector element.

In this example, we select all the values we have recorded within the last `5` minutes for all time series that have the metric name `http_requests_total` and a `job` label set to `prometheus`:

```text
http_requests_total{job="prometheus"}[5m]
```

#### Time durations

Time durations are specified as a number, followed immediately by one of the following units:

- `ms` - milliseconds
- `s` - seconds
- `m` - minutes
- `h` - hours
- `d` - days - assuming a day has always 24h
- `w` - weeks - assuming a week has always 7d
- `y` - years - assuming a year has always 365d

Time durations can be combined, by concatenation. Units must be ordered from the longest to the shortest. A given unit must only appear once in a time duration.

Here are some examples of valid time durations:

```text
5h
1h30m
5m
10s
```

#### Offset modifier

The offset modifier allows changing the time offset for individual instant and range vectors in a query.

For example, the following expression returns the value of `http_requests_total` 5 minutes in the past relative to the current query evaluation time:

```text
http_requests_total offset 5m
```

This returns the 5-minute rate that `http_requests_total` had a week ago:

```text
rate(http_requests_total[5m] offset 1w)
```

For comparisons with temporal shifts forward in time, a negative offset can be specified:

```text
rate(http_requests_total[5m] offset -1w)
```

Note that this allows a query to look ahead of its evaluation time.

#### `@` modifier

The `@` modifier allows changing the evaluation time for individual instant and range vectors in a query. The time supplied to the `@` modifier is a unix timestamp and described with a float literal.

For example, the following expression returns the value of `http_requests_total` at `2021-01-04T07:40:00+00:00`:

```text
http_requests_total @ 1609746000
```

This returns the 5-minute rate that `http_requests_total` had at `2021-01-04T07:40:00+00:00`:

```text
rate(http_requests_total[5m] @ 1609746000)
```

The `@` modifier supports all representation of float literals described above within the limits of int64. It can also be used along with the `offset` modifier where the offset is applied relative to the `@` modifier time irrespective of which modifier is written first. These 2 queries will produce the same result.

```text
http_requests_total @ 1609746000 offset 5m
http_requests_total offset 5m @ 1609746000
```

### Scalar

A **scalar** is a simple numeric floating point value.

### String

A **string** is a simple string value; currently unused.

## Functions

### `increase(v range-vector)`

[↑ `increase(v range-vector)`](https://prometheus.io/docs/prometheus/latest/querying/functions/#increase) calculates the increase in the time series in the range vector. Breaks in monotonicity (such as counter resets due to target restarts) are automatically adjusted for. The increase is extrapolated to cover the full time range as specified in the range vector selector, so that it is possible to get a non-integer result even if a counter increases only by integer increments.

The following example expression returns the number of HTTP requests as measured over the last 5 minutes, per time series in the range vector:

```text
increase(http_requests_total{job="api-server"}[5m])
```

`increase` should only be used with counters and native histograms where the components behave like counters. It is syntactic sugar for `rate(v)` multiplied by the number of seconds under the specified time range window, and should be used primarily for human readability.

### `rate(v range-vector)`

[↑ How Exactly Does PromQL Calculate Rates?](https://promlabs.com/blog/2021/01/29/how-exactly-does-promql-calculate-rates).

The [↑ `rate(m[d])`](https://prometheus.io/docs/prometheus/latest/querying/functions/#rate) function calculates the increase of a counter metric `m` over the given lookbehind window `d` in square brackets and then divides the increase by `d`. The calculation is performed independently per each matching time series `m`. For example, suppose there are `http_requests_total` metrics with `url` label:

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

### `resets(v range-vector)`

The [↑ `resets`](https://prometheus.io/docs/prometheus/latest/querying/functions/#resets) function gives you the number of counter resets over a specified time window:

```text
resets(parser_processed_documents_total[5m])
```

For each input time series, `resets` returns the number of counter resets within the provided time range as an instant vector. Any decrease in the value between two consecutive samples is interpreted as a counter reset.

`resets` should only be used with counters.
