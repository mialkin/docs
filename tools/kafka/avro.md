# Avro

[↑ Avro](https://avro.apache.org/docs) is a data serialization system.

Avro provides:

- Rich data structures
- A compact, fast, binary data format
- A container file, to store persistent data
- Remote procedure call (RPC)
- Simple integration with dynamic languages. Code generation is not required to read or write data files nor to use or implement RPC protocols. Code generation as an optional optimization, only worth implementing for statically typed languages

## Schemas

Avro relies on schemas.

When Avro data is read, the schema used when writing it is always present.

When Avro data is stored in a file, its schema is stored with it, so that files may be processed later by any program.

When Avro is used in RPC, the client and server exchange schemas in the connection handshake.

Avro schemas are defined with JSON.

[↑ Convert any Avro schema to C# model online](https://avroconvertonline.azurewebsites.net).

## Key features

Avro is highly popular in the big data world. It has two key features:

- **compression** — data is well encoded and serialized to byte representation. Using advanced compression algorithms can only improve the size reduction.
- **clear model** — the model of the data is delivered alongside the data itself. What is different, is the format. Model is represented in well-known and human-readable JSON format. It enables backward and forwards compatibility features, which is unique for this level of data compression.

## Avro vs JSON

[↑ AVRO vs JSON](https://www.nidhivichare.com/blog/avro-vs-json).
