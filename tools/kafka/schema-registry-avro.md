# Schema Registry, Avro

## Table of contents

- [Schema Registry, Avro](#schema-registry-avro)
  - [Table of contents](#table-of-contents)
  - [Schema Registry](#schema-registry)
    - [Serde](#serde)
  - [Avro](#avro)
    - [Schemas](#schemas)
    - [Key features](#key-features)
    - [Links](#links)

## Schema Registry

[↑ Schema Registry](https://github.com/confluentinc/schema-registry) is a standalone server process that runs on a machine external to the Kafka brokers. Its job is to maintain a database of all of the schemas that have been written into topics in the cluster for which it is responsible.

Kafka is used as Schema Registry storage backend. The special Kafka topic `<kafkastore.topic>` (default `_schemas`), with a single partition, is used as a highly available write ahead log.

Schema Registry provides serializers that plug into Kafka clients that handle schema storage and retrieval for Kafka messages that are sent in any of the supported formats.

When a producer is configured to use Schema Registry, it calls an API at the Schema Registry REST endpoint and presents the schema of the new message. If it is the same as the last message produced, then the produce may succeed. If it is different from the last message but matches the compatibility rules defined for the topic, the produce may still succeed. But if it is different in a way that violates the compatibility rules, the produce will fail in a way that the application code can detect.

Likewise on the consume side, if a consumer reads a message that has an incompatible schema from the version the consumer code expects, Schema Registry will tell it not to consume the message. Schema Registry doesn't fully automate the problem of schema evolution — that is a challenge in any system regardless of the tooling — but it does make a difficult problem much easier by preventing runtime failures when possible.

- [↑ Schema Registry Overview](https://docs.confluent.io/platform/current/schema-registry/index.html)
- [↑ Schema Evolution and Compatibility](https://docs.confluent.io/platform/current/schema-registry/avro.html#schema-evolution-and-compatibility)
- [↑ Schema Deletion Guidelines](https://docs.confluent.io/platform/current/schema-registry/schema-deletion-guidelines.html#schema-deletion-guidelines)

### Serde

Every Kafka Streams application must provide Serdes serializer/deserializer for the data types of record keys and record values to materialize the data when necessary.

## Avro

[↑ Avro](https://avro.apache.org/docs) is a data serialization system.

Avro provides:

- Rich data structures
- A compact, fast, binary data format
- A container file, to store persistent data
- Remote procedure call (RPC)
- Simple integration with dynamic languages. Code generation is not required to read or write data files nor to use or implement RPC protocols. Code generation as an optional optimization, only worth implementing for statically typed languages

### Schemas

Avro relies on schemas.

When Avro data is read, the schema used when writing it is always present.

When Avro data is stored in a file, its schema is stored with it, so that files may be processed later by any program.

When Avro is used in RPC, the client and server exchange schemas in the connection handshake.

Avro schemas are defined with JSON.

[↑ Convert any Avro schema to C# model online](https://avroconvertonline.azurewebsites.net).

### Key features

Avro is highly popular in the big data world. It has two key features:

- **compression** — data is well encoded and serialized to byte representation. Using advanced compression algorithms can only improve the size reduction.
- **clear model** — the model of the data is delivered alongside the data itself. What is different, is the format. Model is represented in well-known and human-readable JSON format. It enables backward and forwards compatibility features, which is unique for this level of data compression.

### Links

[↑ Why Avro for Kafka Data?](https://www.confluent.io/blog/avro-kafka-data).
