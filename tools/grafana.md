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
  - [Visualizations](#visualizations)
  - [Data sources](#data-sources)
  - [Grafana Loki](#grafana-loki)

## Run

```bash
docker run --detach --publish 3000:3000 --name=grafana grafana/grafana-enterprise
```

## Basics

A **dashboard** is a set of one or more *panels* organized and arranged into one or more *rows*.

You can create a **folder** and move several dashboards in it. Folders help you organize and group dashboards, which is useful when you have many dashboards or multiple teams using the same Grafana instance.

A **panel** is a basic visualization building block in Grafana.

Each panel contains a set of queries to one or more data sources.

Each panel has a query editor specific to the data source selected in the panel. The query editor allows you to build a query that returns the data you want to visualize.

The query defines *what* data you want to display, whereas the visualization defines *how* the data is displayed.

## Connect to Prometheus

**Configuration** (gear icon) → **Data Sources** → `http://host.docker.internal:9090`

## Visualizations

Grafana offers a variety of [↑ visualizations](https://grafana.com/docs/grafana/latest/panels-visualizations/visualizations) to support different use cases.

You can explore [↑ play.grafana.org](https://play.grafana.org) which has a large set of demo dashboards that showcase all the different visualizations.

## Data sources

[↑ Data sources](https://grafana.com/docs/grafana/latest/datasources).

## Grafana Loki

[↑ Grafana Loki](https://grafana.com/docs/loki/latest) is a set of components that can be composed into a fully featured logging stack.

Unlike other logging systems, Loki is built around the idea of only indexing metadata about your logs: labels (just like Prometheus labels). Log data itself is then compressed and stored in chunks in object stores such as S3 or GCS, or even locally on the filesystem. A small index and highly compressed chunks simplifies the operation and significantly lowers the cost of Loki.
