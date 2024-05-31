# GraphQL, gRPC

## Table of contents

- [GraphQL, gRPC](#graphql-grpc)
  - [Table of contents](#table-of-contents)
  - [GraphQL](#graphql)
    - [GraphQL vs REST APIs](#graphql-vs-rest-apis)
  - [gRPC](#grpc)

## GraphQL

**GraphQL** is a query and a manipulation language for APIs and a runtime for fulfilling those queries with your existing data.

A **schema** is what describes the API in full. It's self-documenting and is comprised of *types*. It must have a *root query type*.

A **type** can be anything in GraphQL: *query*, *mutation*, *subscription*, *object*, *enumeration*, *scalar*: id, int, string, boolean, float.

A **resolver** is a way to get a data for a given field. They can resolve to anything: e.g. database, microservice, REST API.

### GraphQL vs REST APIs

From a *developing* perspective GraphQL APIs are *much* harder to create than REST APIs, but, as a consequence, from *consuming* perspective, GraphQL is much better.

There is *nothing wrong* with REST, but:

- REST over-fetches: returning more data than you need
- REST under-fetches: you need to make multiple requests

When to use GraphQL:

- Interactive/real-time
- Mobile apps
- Complex object hierarchy
- Complex queries

When to use REST:

- Non-interactive (system to system)
- Microservices
- Simple object hierarchy
- Repeated, simple queries

## gRPC

**gRPC** is an open source high performance RPC framework.

gRPC uses HTTP/2 for transport, *Protocol Buffers* as the interface description language, and provides features such as authentication, bidirectional streaming and flow control, blocking or non blocking bindings, and cancellation and timeouts. It generates cross-platform client and server bindings for many languages.

Most common usage scenarios include connecting services in microservices style architecture and connect mobile devices, browser clients to backend services.

**Protocol Buffers** or **Protobuf** is a method of serializing structured data. It is useful in developing programs to communicate with each other over a wire or for storing data. The method involves an interface description language that describes the structure of some data and a program that generates source code from that description for generating or parsing a stream of bytes that represents the structured data.

[↑ Introduction to gRPC on .NET Core](https://docs.microsoft.com/en-us/aspnet/core/grpc).

[↑ Troubleshoot gRPC on .NET Core](https://docs.microsoft.com/en-us/aspnet/core/grpc/troubleshoot).

[↑ gRPC JSON transcoding in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/grpc/json-transcoding).

[↑ gRPC JSON transcoding documentation with Swagger / OpenAPI](https://learn.microsoft.com/en-us/aspnet/core/grpc/json-transcoding-openapi).
