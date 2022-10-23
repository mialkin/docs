# Elasticsearch

**Elasticsearch** is a distributed full-text search and analytics engine with an HTTP web interface and schema-free JSON documents.

Elasticsearch provides near real-time search and analytics for all types of data.

[↑ Welcome to Elastic Docs](https://www.elastic.co/guide/index.html).

## Terminology

[↑ Terminology](https://www.elastic.co/guide/en/elastic-stack-glossary/current/terms.html).

A **document** is a JSON object containing data stored in Elasticsearch.

An **ID** is an identifier for a document. Document IDs must be unique within an *index*. See the [↑ `_id`](https://www.elastic.co/guide/en/elasticsearch/reference/master/mapping-id-field.html) field.

A **field** is a key-value pair in document.

An **index** is a collection of JSON documents.

An **indexing** is a process of adding one or more JSON documents to Elasticsearch.

A **mapping** is definition of how a document, its fields, and its metadata are stored in Elasticsearch. Similar to a schema definition.

An **alias** is a secondary name for a group of data streams or indices.

A **query**, is a request for information about data in *data streams* or indices.

A **search** is a set of one or more queries that are combined and sent to Elasticsearch.

A **hit** or **search result** is a document that matches a search's queries and returned in response.

A **data stream** is a named resource used to manage time series data. A data stream stores data across multiple backing indices.

A **time series data** is a series of data points, such as logs, metrics and events, that is indexed in time order. Time series data can be indexed in a data stream, where it can be accessed as a single named resource with the data stored across multiple backing indices. A time series data stream is optimized for indexing metrics data.

A **time series data stream** is a type of data stream optimized for indexing metrics time series data. A TSDS allows for reduced storage size and for a sequence of metrics data points to be considered efficiently as a whole.

## Field data types

[↑ Field data types](https://www.elastic.co/guide/en/elasticsearch/reference/master/mapping-types.html).

Each field has a **field data type**, or **field type**. This type indicates the kind of data the field contains, such as strings or boolean values, and its intended use. For example, you can index strings to both `text` and `keyword` fields. However, `text` field values are analyzed for full-text search while `keyword` strings are left as-is for filtering and sorting.

Field types are grouped by **family**. Types in the same family have exactly the same search behavior but may have different space usage or performance characteristics.

Currently, there are two type families, `keyword` and `text`. Other type families have only a single field type. For example, the boolean type family consists of one field type: `boolean`.

## Text analysis

A **text analysis** is the process of converting unstructured text, like the body of an email or a product description, into a structured format that's optimized for search.

Elasticsearch performs text analysis when indexing or searching `text` fields.

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
