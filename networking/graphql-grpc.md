# GraphQL, RPC, gRPC, protocol buffers

## Table of contents

- [GraphQL, RPC, gRPC, protocol buffers](#graphql-rpc-grpc-protocol-buffers)
  - [Table of contents](#table-of-contents)
  - [GraphQL](#graphql)
    - [GraphQL vs REST APIs](#graphql-vs-rest-apis)
  - [RPC](#rpc)
  - [gRPC](#grpc)
  - [Protocol buffers](#protocol-buffers)
    - [When to use](#when-to-use)
    - [When not to use](#when-not-to-use)

## GraphQL

**GraphQL** is a query and a manipulation language for APIs and a runtime for fulfilling those queries with your existing data.

A **schema** is what describes the API in full. It's self-documenting and is comprised of _types_. It must have a _root query type_.

A **type** can be anything in GraphQL: _query_, _mutation_, _subscription_, _object_, _enumeration_, _scalar_: id, int, string, boolean, float.

A **resolver** is a way to get a data for a given field. They can resolve to anything: e.g. database, microservice, REST API.

### GraphQL vs REST APIs

From a _developing_ perspective GraphQL APIs are _much_ harder to create than REST APIs, but, as a consequence, from _consuming_ perspective, GraphQL is much better.

There is _nothing wrong_ with REST, but:

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

## RPC

A [↑ **remote procedure call**](https://en.wikipedia.org/wiki/Remote_procedure_call) or **RPC** is when a computer program causes a procedure to execute in a different address space, commonly on another computer on a shared computer network, which is written as if it were a normal, local, procedure call, without the programmer explicitly writing the details for the remote interaction.

## gRPC

[↑ **gRPC**](https://grpc.io/docs/what-is-grpc/introduction/) is an open source high performance _remote procedure call_ (RPC) framework.

It can efficiently connect services in and across data centers with pluggable support for load balancing, tracing, health checking and authentication. It is also applicable in last mile of distributed computing to connect devices, mobile applications and browsers to backend services.

gRPC uses HTTP/2 for transport, [protocol buffers](#protocol-buffers) as the interface description language, and provides features such as authentication, bidirectional streaming and flow control, blocking or non blocking bindings, and cancellation and timeouts. It automatically generate idiomatic client and server stubs for your service in a variety of languages and platforms.

[↑ Introduction to gRPC on .NET Core](https://docs.microsoft.com/en-us/aspnet/core/grpc).

[↑ Troubleshoot gRPC on .NET Core](https://docs.microsoft.com/en-us/aspnet/core/grpc/troubleshoot).

[↑ gRPC JSON transcoding in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/grpc/json-transcoding).

[↑ gRPC JSON transcoding documentation with Swagger / OpenAPI](https://learn.microsoft.com/en-us/aspnet/core/grpc/json-transcoding-openapi).

## Protocol buffers

The [↑ **protocol buffers**](https://protobuf.dev/) are language-neutral, platform-neutral extensible mechanisms for serializing structured data.

It's like JSON, except it's smaller and faster, and it generates native language bindings.

Protocol buffers provide a serialization format for packets of typed, structured data that are up to a few megabytes in size. The format is suitable for both ephemeral network traffic and long-term data storage. Protocol buffers can be extended with new information without invalidating existing data or requiring code to be updated.

Protocol buffers are the most commonly-used data format at Google. They are used extensively in inter-server communications as well as for archival storage of data on disk. Protocol buffer messages and services are described by engineer-authored `.proto` files. The following shows an example `message`:

```proto
message Person {
  optional string name = 1;
  optional int32 id = 2;
  optional string email = 3;
}
```

The proto compiler is invoked at build time on `.proto` files to generate code in various programming languages to manipulate the corresponding protocol buffer. Each generated class contains simple accessors for each field and methods to serialize and parse the whole structure to and from raw bytes.

Protocol buffers allow for the seamless support of changes, including the addition of new fields and the deletion of existing fields, to any protocol buffer without breaking existing services.

### When to use

Protocol buffers are ideal for any situation in which you need to serialize structured, record-like, typed data in a language-neutral, platform-neutral, extensible manner. They are most often used for defining communications protocols (together with gRPC) and for data storage.

Some of the advantages of using protocol buffers include:

- Compact data storage
- Fast parsing
- Availability in many programming languages
- Optimized functionality through automatically-generated classes

### When not to use

Protocol buffers do not fit all data. In particular:

- Protocol buffers tend to assume that entire messages can be loaded into memory at once and are not larger than an object graph. For data that exceeds a few megabytes, consider a different solution; when working with larger data, you may effectively end up with several copies of the data due to serialized copies, which can cause surprising spikes in memory usage.
- When protocol buffers are serialized, the same data can have many different binary serializations. You cannot compare two messages for equality without fully parsing them.
- Messages are not compressed. While messages can be zipped or gzipped like any other file, special-purpose compression algorithms like the ones used by JPEG and PNG will produce much smaller files for data of the appropriate type.
- Protocol buffer messages are less than maximally efficient in both size and speed for many scientific and engineering uses that involve large, multi-dimensional arrays of floating point numbers. For these applications, FITS and similar formats have less overhead.
- Protocol buffers are not well supported in non-object-oriented languages popular in scientific computing, such as Fortran and IDL.
- Protocol buffers messages don't inherently self-describe their data, but they have a fully reflective schema that you can use to implement self-description. That is, you cannot fully interpret one without access to its corresponding `.proto` file.
- Protocol buffers are not a formal standard of any organization. This makes them unsuitable for use in environments with legal or other requirements to build on top of standards.
