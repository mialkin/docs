# Elasticsearch commands

## Table of contents

- [Elasticsearch commands](#elasticsearch-commands)
  - [Table of contents](#table-of-contents)
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

## Get cluster information

Get cluster information:

```bash
curl localhost:9200
```

## Concise example

```json
PUT /test
{
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "length": { "type": "integer" }
    }
  }
}
GET test
GET test/_mapping
POST test/_doc
{
  "length": "5"
}
GET test/_search
POST test/_doc
{
  "this_will_produce_error": "5"
}
POST test/_doc/O8j2WoQB6vbHwDshMP1w
{
  "length": "15"
}
DELETE test
```

## Create index

[↑ Create index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html)

```bash
curl -X PUT "localhost:9200/my-index-000001?pretty"
```

```elastic
PUT books_test
```

Create index with dynamic mapping turned off:

```json
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

## Delete index

```text
DELETE books_test
```

## Index document

[↑ Index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html)

```json
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

```json
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

## Get document by ID

[↑ Get API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-get.html)

```text
GET books_test/_doc/5QvnUoQBvyqDobl6CzNM
GET books_test/_doc/1
```

## List indices

List all documents in index:

```bash
curl "localhost:9200/my-index-000001/_search?pretty"
```

## Get mappings

```text
GET books_test/_mapping
```

## Update mapping

## Get index information

[↑ Get information](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-get-index.html) about ore more indices:

```bash
curl "localhost:9200/my-index-000001?pretty"
curl "localhost:9200/*?pretty"
curl "localhost:9200/_all?pretty"
```

```text
GET books_test
```

## Get aliases

[↑ Get aliases](https://www.elastic.co/guide/en/elasticsearch/reference/current/aliases.html):

```bash
curl "localhost:9200/_alias?pretty"
curl "localhost:9200/my-index-000001/_alias?pretty"
curl "http://localhost:9200/_cat/indices?v"
curl "http://localhost:9200/_status"
```
