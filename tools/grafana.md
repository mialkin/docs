# Grafana. Grafana Loki

## Table of contents

- [Grafana. Grafana Loki](#grafana-grafana-loki)
  - [Table of contents](#table-of-contents)
  - [Grafana](#grafana)
  - [Grafana Loki](#grafana-loki)

## Grafana

[↑ Grafana](https://grafana.com/grafana/) is an open source visualization and analytics platform.

A **dashboard** is a set of one or more *panels* organized and arranged into one or more *rows*.

You can create a **folder** and move several dashboards in it. Folders help you organize and group dashboards, which is useful when you have many dashboards or multiple teams using the same Grafana instance.

A **panel** is a basic visualization building block in Grafana.

Each panel contains a set of queries to one or more data sources.

Each panel has a query editor specific to the data source selected in the panel. The query editor allows you to build a query that returns the data you want to visualize.

The query defines *what* data you want to display, whereas the visualization defines *how* the data is displayed.

Grafana offers a variety of [↑ visualizations](https://grafana.com/docs/grafana/latest/panels-visualizations/visualizations) to support different use cases.

You can explore [↑ play.grafana.org](https://play.grafana.org) which has a large set of demo dashboards that showcase all the different visualizations.

Grafana comes with built-in support for many [↑ data sources](https://grafana.com/docs/grafana/latest/datasources).

To connect Grafana to Prometheus go to:
**Configuration** (gear icon) → **Data Sources** → `http://host.docker.internal:9090`

## Grafana Loki

[↑ Grafana Loki](https://grafana.com/docs/loki/latest) is a set of components that can be composed into a fully featured logging stack.

Unlike other logging systems, Loki is built around the idea of only indexing metadata about your logs’ labels (just like Prometheus labels). Log data itself is then compressed and stored in chunks in object stores such as Amazon Simple Storage Service (S3) or Google Cloud Storage (GCS), or even locally on the filesystem.
