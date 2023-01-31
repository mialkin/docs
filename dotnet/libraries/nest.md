# NEST

[↑ NEST](https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/nest.html) is a high level Elasticsearch .NET client, which uses Elasticsearch's json REST API through **Elasticsearch.Net**, the low level .NET client, and exposes all of the endpoints with strong types, using JSON.Net for serialization.

## Installation

```bash
dotnet add package NEST --version 7.17.5
```

The [↑ Elasticsearch v8 client](https://www.elastic.co/guide/en/elasticsearch/client/net-api/current/introduction.html) for .NET is currently in pre-release and not supported for production use. Until the v8 .NET client is generally available, you may use the [↑ v7.17.x client](https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/introduction.html) to communicate with a 8.x Elasticsearch cluster.

[↑ Release notes v8.0.0](https://www.elastic.co/guide/en/elasticsearch/client/net-api/current/release-notes-8.0.0.html)

## Table of contents

- [NEST](#nest)
  - [Installation](#installation)
  - [Table of contents](#table-of-contents)
  - [Why two clients](#why-two-clients)
  - [Inferred .NET type mapping](#inferred-net-type-mapping)
  - [Indices](#indices)
    - [List](#list)
    - [Get](#get)
    - [Create](#create)
    - [Delete](#delete)
  - [Documents](#documents)
    - [Create](#create-1)
    - [Get by ID](#get-by-id)
    - [Delete by ID](#delete-by-id)
  - [Mappings](#mappings)
    - [Get mapping](#get-mapping)
  - [Search documents](#search-documents)
    - [Match all](#match-all)
    - [Term](#term)
    - [Terms](#terms)
    - [Terms `AND`](#terms-and)
    - [Terms `OR`](#terms-or)
  - [Links](#links)

## Why two clients

NEST comes with a strongly typed query DSL that maps 1 to 1 with the Elasticsearch query DSL. It takes advantage of specific .NET features to provide higher level abstractions such as auto mapping of CLR types. Internally, NEST uses and still exposes the low level Elasticsearch.Net client, providing access to the power of NEST and allowing users to drop down to the low level client when wishing to.

Elasticsearch.Net is a low level, dependency free client that has no opinions about how you build and represent your requests and responses. It has abstracted enough so that all the Elasticsearch API endpoints are represented as methods but not too much to get in the way of how you want to build your JSON/request/response objects. It also comes with built in, configurable/overridable cluster failover retry mechanisms. Elasticsearch is elastic so why not your client?

## Inferred .NET type mapping

[↑ Inferred .NET type mapping](https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/auto-map.html#inferred-dotnet-type-mapping)

## Indices

### List

```csharp
// GET /_cat/indices?v
var result = await _elasticClient.Indices.GetAsync(new GetIndexRequest(Indices.All));
var indexNames = result.Indices.Select(x => x.Key.Name);
```

### Get

```csharp
// GET indexName
var result = await _elasticClient.Indices.GetAsync(indexName);
var index = result.Indices.First();
var name = index.Key.Name;
var settings = index.Value.Settings;
var mappings = index.Value.Mappings;
var aliases = index.Value.Aliases;
return Ok(new { name, aliases, mappings, settings });
```

### Create

```csharp
// PUT indexName
await _elasticClient.Indices.CreateAsync(indexName);
```

### Delete

```csharp
// DELETE indexName
await _elasticClient.Indices.DeleteAsync(indexName);
```

## Documents

### Create

```csharp
// POST products/_doc
// {
// }
// or:
// PUT products/_doc/bd971fd3-9a7b-4202-a2a4-5d143242d453
// {
//     "id": "bd971fd3-9a7b-4202-a2a4-5d143242d453",
//     "name": "chair",
//     "isActive": true,
//     "price": 19.99,
//     "createdOn": "2023-02-13T00:00:00"
// }
await _elasticClient.IndexAsync(document, x => x.Refresh(Refresh.WaitFor).Index(indexName));
```

### Get by ID

```csharp
// GET products/_doc/id
var result = await _elasticClient.GetAsync<ProductDto>(id, x => x.Index("products"));
var dto = result.Source;
```

### Delete by ID

```csharp
// DELETE products/_doc/id
await _elasticClient.DeleteAsync(new DeleteRequest("products", id));
```

## Mappings

### Get mapping

```csharp
// GET products/_mapping
```

## Search documents

### Match all

```csharp
// GET products/_search
// or:
// GET products/_search
// {
//     "query": {
//         "match_all": {}
//     }
// }
var result = await _elasticClient.SearchAsync<ProductDto>(x => x.Index("products").MatchAll());
var documents = result.Documents;
```

### Term

```csharp
// GET products/_search
// {
//     "query": {
//         "term": {
//             "price": {
//                 "value": 19.99
//             }
//         }
//     }
// }
var result = await _elasticClient.SearchAsync<ProductDto>(search =>
    search
        .Index("products")
        .Query(query =>
            query.Term(x => x.Field(y => y.Price).Value(19.99)))
);

var documents = result.Documents;
```

### Terms

```csharp
// GET products/_search
// {
//     "query": {
//         "terms": {
//             "price": [
//             19.99,
//             10.99
//                 ]
//         }
//     }
// }
var result = await _elasticClient.SearchAsync<ProductDto>(search =>
    search
        .Index("products")
        .Query(query =>
            query.Terms(x => x.Field(y => y.Price).Terms(19.99m, 10.99m)))
);

var documents = result.Documents;
```

### Terms `AND`

```csharp
// GET products/_search
// {
//     "query": {
//         "bool": {
//             "must": [
//             {
//                 "terms": {
//                     "price": [
//                     19.99,
//                     10.99
//                         ]
//                 }
//             },
//             {
//                 "term": {
//                     "name": {
//                         "value": "pillow"
//                     }
//                 }
//             }
//             ]
//         }
//     }
// }
var result = await _elasticClient.SearchAsync<ProductDto>(search =>
    search
        .Index("products")
        .Query(query => query
            .Terms(x => x.Field(y => y.Price).Terms(prices)) && query
            .Term(x => x.Field(y => y.Name).Value(name)))
);

var documents = result.Documents;
```

### Terms `OR`

```csharp
// GET products/_search
// {
//     "query": {
//         "bool": {
//             "should": [
//             {
//                 "terms": {
//                     "price": [
//                     19.99
//                         ]
//                 }
//             },
//             {
//                 "term": {
//                     "name": {
//                         "value": "chair"
//                     }
//                 }
//             }
//             ]
//         }
//     }
// }
var result = await _elasticClient.SearchAsync<ProductDto>(search =>
    search
        .Index("products")
        .Query(query => query
            .Terms(x => x.Field(y => y.Price).Terms(prices)) || query
            .Term(x => x.Field(y => y.Name).Value(name)))
);

var documents = result.Documents;
```

## Links

- [↑ Deep Dive into Querying Elasticsearch. Filter vs Query. Full-text search](https://towardsdatascience.com/deep-dive-into-querying-elasticsearch-filter-vs-query-full-text-search-b861b06bd4c0)
- [↑ ELASTIC SEARCH - QUERIES, AGGREGATIONS AND FILTERS USING ASP.NET AND NEST](https://www.methylium.com/articles/elastic-search-filters/)
- [↑ Boolean query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html)