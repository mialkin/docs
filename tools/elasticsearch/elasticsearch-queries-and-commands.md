# Elasticsearch queries and commands

## Table of contents

- [Elasticsearch queries and commands](#elasticsearch-queries-and-commands)
  - [Table of contents](#table-of-contents)
  - [Queries](#queries)
    - [`match_all`](#match_all)
    - [`match_none`](#match_none)
    - [Match query vs term query](#match-query-vs-term-query)
  - [Commands](#commands)
    - [Get cluster information](#get-cluster-information)
    - [Concise example](#concise-example)
    - [Create index](#create-index)
    - [Delete index](#delete-index)
    - [Index document](#index-document)
    - [Get document by ID](#get-document-by-id)
    - [List indices](#list-indices)
    - [Get mappings](#get-mappings)
    - [Update mapping](#update-mapping)
    - [Get index information](#get-index-information)
    - [Get aliases](#get-aliases)

## Queries

[↑ Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/8.4/query-dsl.html)

### `match_all`

Match all documents, giving them all a `_score` of `1.0`:

```dsl
GET my-index-000001/_search
{
  "query": {
    "match_all": {}
  }
}
```

The `_score` can be changed with the `boost` parameter:

```dsl
GET my-index-000001/_search
{
  "query": {
    "match_all": { "boost" : 1.2 }
  }
}
```

### `match_none`

This is the inverse of the `match_all` query, which matches no documents:

```dsl
GET my-index-000001/_search
{
  "query": {
    "match_all": {}
  }
}
```

```dsl
GET my-index-000001/_search
{
  "query": {
    "query_string": {
      "query": "apples"
    }
  }
}
```

Filter:

```dsl
GET users/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_all": {}
        }
      ],
      "filter": [
        {
          "term": {
            "id": "32211930947"
          }
        }
      ]
    }
  }
}
```

Update:

```dsl
POST users/_update_by_query
{
  "query": {
    "range": {
      "registrationDate": {
        "lt": "2022-12-07T00:00:00"
      }
    }
  },
  "script": {
    "source": "ctx._source.users = null",
    "lang": "painless"
  }
}
```

### Match query vs term query

Same as `text` and `keyword`, the difference between Match Query and Term Query is that the query in Match Query will get analyzed into terms first, while the query in Term Query will not.

[↑ Elasticsearch: Text vs. Keyword](https://codecurated.com/blog/elasticsearch-text-vs-keyword)

## Commands

### Get cluster information

Get cluster information:

```bash
curl localhost:9200
```

### Concise example

```dsl
POST test/_doc/1
{
  "name": "An Awesome Book 2222",
  "tags": [{ "name": "best-seller" }, { "name": "summer-sale" }],
  "authors": [
    { "name": "Gustavo Llermaly", "age": "32", "country": "Chile" },
    { "name": "John Doe", "age": "20", "country": "USA" }
  ]
}

PUT /test
{
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "title": { "type": "text" },
      "length": { "type": "integer" },
      "width": { "type": "integer"}
    }
  }
}
GET test
GET test/_mapping
POST test/_doc/1
{
  "length": "5"
}
GET test/_search
# Replaces entirely existing document:
POST test/_doc/1
{
  "title": "The Title"
}
POST test/_update/1
{
    "doc" : {
        "length": "100",
        "width": "500"
    }
}
DELETE test/_doc/1
DELETE test
```

### Create index

[↑ Create index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html)

```bash
curl -X PUT "localhost:9200/my-index-000001?pretty"
```

```elastic
PUT books_test
```

Create index with dynamic mapping turned off:

```dsl
PUT /test
{
  "mappings": {
    "dynamic": false,
    "properties": {
      "length": { "type": "integer" }
    }
  }
}
```

[↑ `dynamic` mapping parameter](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/dynamic.html)

### Delete index

```text
DELETE books_test
```

### Index document

[↑ Index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html)

```dsl
POST books_test/_doc
{
  "name": "An Awesome Book",
  "tags": [{ "name": "best-seller" }, { "name": "summer-sale" }],
  "authors": [
    { "name": "Gustavo Llermaly", "age": "32", "country": "Chile" },
    { "name": "John Doe", "age": "20", "country": "USA" }
  ]
}
```

Specify document ID explicitly:

```dsl
POST books_test/_doc/1
{
  "name": "An Awesome Book",
  "tags": [{ "name": "best-seller" }, { "name": "summer-sale" }],
  "authors": [
    { "name": "Gustavo Llermaly", "age": "32", "country": "Chile" },
    { "name": "John Doe", "age": "20", "country": "USA" }
  ]
}
```

If document ID is not specified explicitly, then a unique inside index like `5QvnUoQBvyqDobl6CzNM` will be created automatically.

You can index a new JSON document with the `_doc` or `_create` resource. Using `_create` guarantees that the document is only indexed if it does not already exist. To update an existing document, you must use the `_doc` resource.

### Get document by ID

[↑ Get API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-get.html)

```text
GET books_test/_doc/5QvnUoQBvyqDobl6CzNM
GET books_test/_doc/1
```

### List indices

List all documents in index:

```bash
curl "localhost:9200/my-index-000001/_search?pretty"
```

### Get mappings

```text
GET books_test/_mapping
```

### Update mapping

### Get index information

[↑ Get information](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-index.html) about ore more indices:

```bash
curl "localhost:9200/my-index-000001?pretty"
curl "localhost:9200/*?pretty"
curl "localhost:9200/_all?pretty"
```

```text
GET books_test
```

### Get aliases

[↑ Get aliases](https://www.elastic.co/guide/en/elasticsearch/reference/current/aliases.html):

```bash
curl "localhost:9200/_alias?pretty"
curl "localhost:9200/my-index-000001/_alias?pretty"
curl "http://localhost:9200/_cat/indices?v"
curl "http://localhost:9200/_status"
```
