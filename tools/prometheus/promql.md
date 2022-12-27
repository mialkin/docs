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
