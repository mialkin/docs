# Elasticsearch

**Elasticsearch** is a distributed full-text search and analytics engine with an HTTP web interface and schema-free JSON documents.

Elasticsearch provides near real-time search and analytics for all types of data.

[↑ Welcome to Elastic Docs](https://www.elastic.co/guide/index.html).

## Table of contents

- [Elasticsearch](#elasticsearch)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Lucene](#lucene)
    - [Shards](#shards)
    - [Replicas](#replicas)
  - [Terminology](#terminology)
  - [Field data types](#field-data-types)
  - [Mapping](#mapping)
  - [Update documents](#update-documents)
  - [Search documents](#search-documents)
    - [Boolean query](#boolean-query)
  - [Inverted index](#inverted-index)
  - [Text analysis](#text-analysis)

## Installation

The `.env` file:

```env
ELASTICSEARCH_PORT=2020
DEJAVU_PORT=2030
KIBANA_PORT=2040
```

The `docker-compose.yml` file:

```yml
services:
  elasticsearch:
    image: elasticsearch:7.17.6
    container_name: elasticsearch
    environment:
      discovery.type: single-node
      xpack.security.enabled: "false"
      http.cors.enabled: "true"
      http.cors.allow-origin: "http://localhost:${DEJAVU_PORT}"
      http.cors.allow-headers: X-Requested-With,X-Auth-Token,Content-Type,Content-Length,Authorization
      http.cors.allow-credentials: "true"
      ELASTIC_PASSWORD: "elasticsearch"
    ports:
      - "${ELASTICSEARCH_PORT}:9200"
  kibana:
    image: kibana:7.17.6
    container_name: kibana
    environment:
      ELASTICSEARCH_HOSTS: '["http://elasticsearch:9200"]'
    ports:
      - "${KIBANA_PORT}:5601"
    depends_on:
      - elasticsearch

  dejavu:
    image: appbaseio/dejavu:3.3.0
    container_name: dejavu
    ports:
      - "${DEJAVU_PORT}:1358"
    depends_on:
      - elasticsearch
```

## Lucene

Elasticsearch is built over [↑ Lucene Core](https://lucene.apache.org/core) Java library. Each *shard* that gets created in Elasticsearch is a separate Lucene instance.

Lucene provides powerful indexing and search features, as well as spellchecking, hit highlighting and advanced analysis/tokenization capabilities.

Though it's Lucene who is doing the actual work beneath, Elasticsearch provides a convenient layer over Lucene:

- JSON based REST API to refer to Lucene features
- Distributed system on top of Lucene. A distributed system is not something Lucene is aware of or built for
- Thread-pool, queues, node/cluster monitoring API, data monitoring API, cluster management, etc
- Powerful DSL: JSON interface for reading and writing queries on top of Lucene. You can write complex queries without knowing Lucene syntax

### Shards

Put simply, **shards** are a single Lucene index. They are the building blocks of Elasticsearch and what facilitate its scalability.

### Replicas

**Replicas** are Elasticsearch fail-safe mechanisms and are basically copies of your index's shards. This is a useful backup system for a rainy day — or, in other words, when a node crashes. Replicas also serve read requests, so adding replicas can help to increase search performance.

To ensure high availability, replicas are not placed on the same node as the original shards (called the "primary" shard) from which they were replicated. For example, Shard 1 might share Node A with Replica Shard 2, while Node B hosts Shard 2 and Replica Shard 1.

Like with shards, the number of replicas can be defined per index when the index is created. Unlike shards, however, you may change the number of replicas anytime after the index is created. In other words, you can have more than one replica for each shard, but there is a balance to be struck between having too few and too many shards. More shards result in more overhead and resource usage, as well as can affect performance and speed.

## Terminology

[↑ Terminology](https://www.elastic.co/guide/en/elastic-stack-glossary/current/terms.html)

A **document** is a JSON object containing data stored in Elasticsearch.

An **ID** is an identifier for a document. Document IDs must be unique within an *index*. See the [↑ `_id`](https://www.elastic.co/guide/en/elasticsearch/reference/master/mapping-id-field.html) field.

A **field** is a key-value pair in document.

An **index** is a collection of JSON documents.

An **indexing** is a process of adding one or more JSON documents to Elasticsearch.

An **alias** is a secondary name for a group of data streams or indices.

A **data stream** is a named resource used to manage time series data. A data stream stores data across multiple backing indices.

A **time series data** is a series of data points, such as logs, metrics and events, that is indexed in time order. Time series data can be indexed in a data stream, where it can be accessed as a single named resource with the data stored across multiple backing indices. A time series data stream is optimized for indexing metrics data.

A **time series data stream** is a type of data stream optimized for indexing metrics time series data. A TSDS allows for reduced storage size and for a sequence of metrics data points to be considered efficiently as a whole.

## Field data types

[↑ Field data types](https://www.elastic.co/guide/en/elasticsearch/reference/master/mapping-types.html)

Each field has a **field data type**, or **field type**. This type indicates the kind of data the field contains, such as strings or boolean values, and its intended use.

Field types are grouped by **family**. Types in the same family have exactly the same search behavior but may have different space usage or performance characteristics. Currently, there are two type families: `keyword` and `text`. Other type families have only a single field type. For example, the boolean type family consists of one field type: `boolean`.

## Mapping

A **mapping** is definition of how a document, its fields, and its metadata are stored and indexed in Elasticsearch. Similar to a schema definition.

[↑ Auto mapping](https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/auto-map.html#auto-map)

[↑ Mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html)

## Update documents

Documents in Elasticsearch are *immutable*; we cannot change them. Instead, if we need to update an existing document, we *reindex* it:

```csharp
PUT products/_doc/bd971fd3-9a7b-4202-a2a4-5d143242d453
{
    "id": "bd971fd3-9a7b-4202-a2a4-5d143242d453",
    "name": "chair",
    "isActive": true,
    "price": 19.99,
    "createdOn": "2023-02-13T00:00:00"
}
```

In the response, we can see that Elasticsearch has incremented the `_version` number:

```csharp
{
    "_index" :   "products",
    "_type" :    "product",
    "_id" :      "bd971fd3-9a7b-4202-a2a4-5d143242d453",
    "_version" : 2,
    "created":   false 
}
```

The `created` flag is set to `false` because a document with the same index, type, and ID already existed.

Internally, Elasticsearch has marked the old document as deleted and added an entirely new document. The old version of the document doesn’t disappear immediately, although you won’t be able to access it. Elasticsearch cleans up deleted documents in the background as you continue to index more data.

## Search documents

[↑ Search your data](https://www.elastic.co/guide/en/elasticsearch/reference/master/search-your-data.html)

A **query**, is a request for information about data in *data streams* or indices.

A **search** is a set of one or more queries that are combined and sent to Elasticsearch.

A **hit** or **search result** is a document that matches a search's queries and returned in response.

You can use the [↑ search API](https://www.elastic.co/guide/en/elasticsearch/reference/master/search-search.html) to search and aggregate data stored in Elasticsearch data streams or indices. The API's query request body parameter accepts queries written in Query DSL.

[↑ Painless scripting language](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-scripting-painless.html)

### Boolean query

[↑ Boolean query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html).

## Inverted index

[↑ Inverted index](https://www.elastic.co/guide/en/elasticsearch/guide/current/inverted-index.html)

## Text analysis

[↑ Anatomy of an analyzer](https://www.elastic.co/guide/en/elasticsearch/reference/master/analyzer-anatomy.html).

A **text** is an unstructured content, such as a product description or log message.

A **text analysis** is the process of converting unstructured text into a structured format that's optimized for search.

Elasticsearch performs text analysis when indexing or searching `text` fields.

A **token** or a **term** is a chunk of unstructured text that's been optimized for search. In most cases, tokens are individual words.

A **tokenization** is a process of breaking a text down into tokens.

A **normalization** is a process of bringing tokens into a standard format.

An **analyzer** is a package which contains three lower-level building blocks:

- *character filters*
- *tokenizer*
- *token filters*.

A **character filter** is a thing that receives the original text as a stream of characters and can transform the stream by adding, removing, or changing characters.

For example, [↑ HTML strip character filter](https://www.elastic.co/guide/en/elasticsearch/reference/master/analysis-htmlstrip-charfilter.html) strips HTML elements from a text and replaces HTML entities with their decoded value, e.g, replaces `&amp;` with `&`.

An analyzer may have zero or more character filters, which are applied in order.

A **tokenizer** is a thing that receives a stream of characters, breaks it up into individual tokens, and outputs a stream of tokens.

For example, a [↑ whitespace tokenizer](https://www.elastic.co/guide/en/elasticsearch/reference/master/analysis-whitespace-tokenizer.html) breaks text into tokens whenever it sees any whitespace.

The tokenizer is also responsible for recording the order or *position* of each term and the start and end character offsets of the original word which the term represents.

An analyzer must have exactly one tokenizer.

A **token filter** is a thing that receives the token stream and may add, remove, or change tokens.

For example, a [↑ lowercase token filter](https://www.elastic.co/guide/en/elasticsearch/reference/master/analysis-lowercase-tokenfilter.html) converts all tokens to lowercase, a stop token filter removes common words, *stop words*, like the from the token stream, and a synonym token filter introduces synonyms into the token stream.

Token filters are not allowed to change the position or character offsets of each token.

An analyzer may have zero or more token filters, which are applied in order.
