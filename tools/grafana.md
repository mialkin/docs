# Grafana

**Grafana** is an open source visualization and analytics platform.

Grafana page at localhost is <http://localhost:3000>.

[↑ Grafana fundamentals](https://grafana.com/tutorials/grafana-fundamentals).

[↑ Grafana tutorials](https://grafana.com/tutorials).

## Table of contents

- [Grafana](#grafana)
  - [Table of contents](#table-of-contents)
  - [Run](#run)
  - [Basics](#basics)
  - [Connect to Prometheus](#connect-to-prometheus)
  - [Grafana Loki](#grafana-loki)

## Run

```bash
docker run -d -p 3000:3000 --name=grafana grafana/grafana-enterprise
```

## Basics

A **dashboard** is a set of one or more *panels* organized and arranged into one or more *rows*.

A **panel** is a basic visualization building block in Grafana. Each panel has a query editor specific to the data source selected in the panel. The query editor allows you to build a query that returns the data you want to visualize.

Every panel consists of a *query* and a *visualization*. The query defines *what* data you want to display, whereas the visualization defines *how* the data is displayed.

## Connect to Prometheus

**Configuration** (gear icon) → **Data Sources** → `http://host.docker.internal:9090`

## Grafana Loki

[↑ Grafana Loki](https://grafana.com/docs/loki/latest) is a set of components that can be composed into a fully featured logging stack.

Unlike other logging systems, Loki is built around the idea of only indexing metadata about your logs: labels (just like Prometheus labels). Log data itself is then compressed and stored in chunks in object stores such as S3 or GCS, or even locally on the filesystem. A small index and highly compressed chunks simplifies the operation and significantly lowers the cost of Loki.
