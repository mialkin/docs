# NEST

[↑ NEST](https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/nest.html) is a high level Elasticsearch .NET client, which uses Elasticsearch's json REST API through **Elasticsearch.Net**, the low level .NET client, and exposes all of the endpoints with strong types, using JSON.Net for serialization.

## Installation

```bash
dotnet add package NEST --version 7.17.5
```

The [↑ Elasticsearch v8 client](https://www.elastic.co/guide/en/elasticsearch/client/net-api/current/introduction.html) for .NET is currently in pre-release and not supported for production use. Until the v8 .NET client is generally available, you may use the [↑ v7.17.x client](https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/introduction.html) to communicate with a 8.x Elasticsearch cluster.

[↑ Release notes v8.0.0](https://www.elastic.co/guide/en/elasticsearch/client/net-api/current/release-notes-8.0.0.html)

## Why two clients

NEST comes with a strongly typed query DSL that maps 1 to 1 with the Elasticsearch query DSL. It takes advantage of specific .NET features to provide higher level abstractions such as auto mapping of CLR types. Internally, NEST uses and still exposes the low level Elasticsearch.Net client, providing access to the power of NEST and allowing users to drop down to the low level client when wishing to.

Elasticsearch.Net is a low level, dependency free client that has no opinions about how you build and represent your requests and responses. It has abstracted enough so that all the Elasticsearch API endpoints are represented as methods but not too much to get in the way of how you want to build your JSON/request/response objects. It also comes with built in, configurable/overridable cluster failover retry mechanisms. Elasticsearch is elastic so why not your client?

## Inferred .NET type mapping

[↑ Inferred .NET type mapping](https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/auto-map.html#inferred-dotnet-type-mapping)
