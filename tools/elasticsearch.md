# Elasticsearch

**Elasticsearch** is a distributed full-text search and analytics engine with an HTTP web interface and schema-free JSON documents.

Elasticsearch provides near real-time search and analytics for all types of data.

[↑ Welcome to Elastic Docs](https://www.elastic.co/guide/index.html).

## Terminology

A **document** is a JSON object containing data stored in Elasticsearch.

An **ID** is an identifier for a document. Document IDs must be unique within an *index*. See the [↑ `_id`](https://www.elastic.co/guide/en/elasticsearch/reference/master/mapping-id-field.html) field.

An **index** is a collection of JSON documents.

A **query**, is a request for information about data in *data streams* or indices.

A **data stream** is a named resource used to manage time series data. A data stream stores data across multiple backing indices.

An **alias** is a secondary name for a group of data streams or indices.

A **time series data** is a series of data points, such as logs, metrics and events, that is indexed in time order. Time series data can be indexed in a data stream, where it can be accessed as a single named resource with the data stored across multiple backing indices. A time series data stream is optimized for indexing metrics data.

A **time series data stream** is a type of data stream optimized for indexing metrics time series data. A TSDS allows for reduced storage size and for a sequence of metrics data points to be considered efficiently as a whole.

[↑ Terminology](https://www.elastic.co/guide/en/elastic-stack-glossary/current/terms.html)

## Commands

List all indexes:

```bash
curl http://localhost:9200/_aliases
# or:
curl http://localhost:9200/_cat/indices?v
# or:
curl http://localhost:9200/_status
```

List all documents in index:

```bash
curl http://localhost:9200:/smaple/_search
```

Query using URL parameters:

```bash
curl http://localhost:9200:/smaple/_search
```
