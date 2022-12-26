# OpenTelemetry

[↑ OpenTelemetry](https://opentelemetry.io) is a collection of tools, APIs, and SDKs used to instrument, generate, collect, and export telemetry data: metrics, logs, and traces.

A **telemetry** is a feedback from a system that tells what's going on in there.

## Table of contents

- [OpenTelemetry](#opentelemetry)
  - [Table of contents](#table-of-contents)
  - [Telemetry categories supported by OpenTelemetry](#telemetry-categories-supported-by-opentelemetry)
  - [OTEL Collector](#otel-collector)
    - [When to use a collector](#when-to-use-a-collector)

## Telemetry categories supported by OpenTelemetry

A **signal** is a category of telemetry that is supported by the OpenTelemetry specification.

Currently supported signals:

- Traces
- Metrics
- Logs
- Baggage

## OTEL Collector

The [↑ OpenTelemetry Collector](https://opentelemetry.io/docs/collector/getting-started) offers a vendor-agnostic implementation of how to receive, process and export telemetry data. It removes the need to run, operate, and maintain multiple agents/collectors. This works with improved scalability and supports open-source observability data formats (e.g. Jaeger, Prometheus, Fluent Bit, etc.) sending to one or more open-source or commercial back-ends. The local Collector agent is the default location to which instrumentation libraries export their telemetry data.

### When to use a collector

For most language specific instrumentation libraries you have exporters for popular backends and *OTLP*. You might wonder, under what circumstances does one use a collector to send data, as opposed to having each service send directly to the backend?

> **OTLP** stands for [↑ OpenTelemetry protocol](https://opentelemetry.io/docs/reference/specification/protocol)

For trying out and getting started with OpenTelemetry, sending your data directly to a backend is a great way to get value quickly. Also, in a development or small-scale environment you can get decent results without a collector.

However, in general we recommend using a collector alongside your service, since it allows your service to offload data quickly and the collector can take care of additional handling like retries, batching, encryption or even sensitive data filtering.

It is also easier to [↑ setup a collector](https://opentelemetry.io/docs/collector/getting-started) than you might think: the default OTLP exporters in each language assume a local collector endpoint, so you'd start up a collector and you'd just start getting telemetry.
