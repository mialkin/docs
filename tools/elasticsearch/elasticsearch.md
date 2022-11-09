# Elasticsearch

**Elasticsearch** is a distributed full-text search and analytics engine with an HTTP web interface and schema-free JSON documents.

Elasticsearch provides near real-time search and analytics for all types of data.

[↑ Welcome to Elastic Docs](https://www.elastic.co/guide/index.html)

## Table of contents

- [Elasticsearch](#elasticsearch)
  - [Table of contents](#table-of-contents)
  - [Lucene](#lucene)
  - [Terminology](#terminology)
  - [Field data types](#field-data-types)
  - [Mapping](#mapping)
  - [Searching data](#searching-data)
    - [Queries](#queries)
      - [`match_all`](#match_all)
      - [`match_none`](#match_none)
      - [Match query vs term query](#match-query-vs-term-query)
  - [Ingest pipeline](#ingest-pipeline)
  - [Text analysis](#text-analysis)

## Lucene

Elasticsearch is built over [↑ Lucene Core](https://lucene.apache.org/core) Java library. Each shard that gets created in Elasticsearch is a separate Lucene instance.

Lucene provides powerful indexing and search features, as well as spellchecking, hit highlighting and advanced analysis/tokenization capabilities.

Though it's Lucene who is doing the actual work beneath, Elasticsearch provides a convenient layer over Lucene:

- JSON based REST API to refer to Lucene features
- Distributed system on top of Lucene. A distributed system is not something Lucene is aware of or built for
- Thread-pool, queues, node/cluster monitoring API, data monitoring API, cluster management, etc
- Powerful DSL: JSON interface for reading and writing queries on top of Lucene. You can write complex queries without knowing Lucene syntax

## Terminology

[↑ Terminology](https://www.elastic.co/guide/en/elastic-stack-glossary/current/terms.html)

A **document** is a JSON object containing data stored in Elasticsearch.

An **ID** is an identifier for a document. Document IDs must be unique within an *index*. See the [↑ `_id`](https://www.elastic.co/guide/en/elasticsearch/reference/master/mapping-id-field.html) field.

A **field** is a key-value pair in document.

An **index** is a collection of JSON documents.

An **indexing** is a process of adding one or more JSON documents to Elasticsearch.

A **mapping** is definition of how a document, its fields, and its metadata are stored in Elasticsearch. Similar to a schema definition.

An **alias** is a secondary name for a group of data streams or indices.

A **data stream** is a named resource used to manage time series data. A data stream stores data across multiple backing indices.

A **time series data** is a series of data points, such as logs, metrics and events, that is indexed in time order. Time series data can be indexed in a data stream, where it can be accessed as a single named resource with the data stored across multiple backing indices. A time series data stream is optimized for indexing metrics data.

A **time series data stream** is a type of data stream optimized for indexing metrics time series data. A TSDS allows for reduced storage size and for a sequence of metrics data points to be considered efficiently as a whole.

## Field data types

[↑ Field data types](https://www.elastic.co/guide/en/elasticsearch/reference/master/mapping-types.html)

Each field has a **field data type**, or **field type**. This type indicates the kind of data the field contains, such as strings or boolean values, and its intended use.

Field types are grouped by **family**. Types in the same family have exactly the same search behavior but may have different space usage or performance characteristics. Currently, there are two type families: `keyword` and `text`. Other type families have only a single field type. For example, the boolean type family consists of one field type: `boolean`.

## Mapping

**Mapping** is the process of defining how a document, and the fields it contains, are stored and indexed.

[↑ Mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html)

[↑ Auto mapping](https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/auto-map.html#auto-map)

## Searching data

[↑ Search your data](https://www.elastic.co/guide/en/elasticsearch/reference/master/search-your-data.html)

A **query**, is a request for information about data in *data streams* or indices.

A **search** is a set of one or more queries that are combined and sent to Elasticsearch.

A **hit** or **search result** is a document that matches a search's queries and returned in response.

You can use the [↑ search API](https://www.elastic.co/guide/en/elasticsearch/reference/master/search-search.html) to search and aggregate data stored in Elasticsearch data streams or indices. The API's query request body parameter accepts queries written in Query DSL.

[↑ Painless scripting language](https://www.elastic.co/guide/en/elasticsearch/reference/master/modules-scripting-painless.html)

### Queries

[↑ Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/query-dsl.html)

#### `match_all`

Match all documents, giving them all a `_score` of `1.0`:

```json
GET my-index-000001/_search
{
  "query": {
    "match_all": {}
  }
}
```

The `_score` can be changed with the `boost` parameter:

```json
GET my-index-000001/_search
{
  "query": {
    "match_all": { "boost" : 1.2 }
  }
}
```

#### `match_none`

This is the inverse of the `match_all` query, which matches no documents:

```json
GET my-index-000001/_search
{
  "query": {
    "match_all": {}
  }
}
```

#### Match query vs term query

Same as `text` and `keyword`, the difference between Match Query and Term Query is that the query in Match Query will get analyzed into terms first, while the query in Term Query will not.

[↑ Elasticsearch: Text vs. Keyword](https://codecurated.com/blog/elasticsearch-text-vs-keyword)

## Ingest pipeline

https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html

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

For example, a [↑ lowercase token filter](https://www.elastic.co/guide/en/elasticsearch/reference/master/analysis-lowercase-tokenfilter.html) converts all tokens to lowercase, a stop token filter removes common words (*stop words*) like the from the token stream, and a synonym token filter introduces synonyms into the token stream.

Token filters are not allowed to change the position or character offsets of each token.

An analyzer may have zero or more token filters, which are applied in order.
