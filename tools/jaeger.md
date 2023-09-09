# Jaeger

[↑ Jaeger](https://github.com/jaegertracing/jaeger) is an open-source distributed tracing platform. It consists of instrumentation SDKs, a backend for data collection and storage, a UI for visualizing the data, and Spark/Flink framework for aggregate trace analysis.

The Jaeger data model is compatible with OpenTracing — which is a specification that defines how the collected tracing data would look, as well as libraries of implementations in different languages.

As most other distributed tracing systems, Jaeger works with *spans* and *traces*, as defined in the OpenTracing specification.

A **span** represents a unit of work in an application (HTTP request, call to a database, etc) and is Jaeger's most basic unit of work. A span must have an operation name, start time, and duration.

A **trace** is a collection/list of spans connected in a parent/child relationship and can also be thought of as a directed acyclic graph of spans. Traces specify how requests are propagated through application services and other components.

## Links

- [↑ Jaeger Tracing: A Friendly Guide for Beginners
](https://medium.com/jaegertracing/jaeger-tracing-a-friendly-guide-for-beginners-7b53a4a568ca).
