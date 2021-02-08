# GraphQL

**GraphQL** is a query and a manipulation language for APIs and a runtime for fulfilling those queries with your existing data.

## Core concepts

* **Schema** is what describes the API in full. It's self-documenting and is comprised of *types*. It must have a *root query type*.
* **Types** can be anything in GraphQL: *query*, *mutation*, *subscription*, *object*, *enumeration*, *scalar* (id, int, string, boolean, float).
* **Resolvers** is a way to get a data for a given field. They can resolve to anything (e.g. database, microservice, REST API).

## GraphQL vs REST

There is *nothing wrong* with REST, but:

* REST over-fetches: returning more data than you need
* REST under-fetches: you need to make multiple requests

When to use GraphQL:

* Interactive/real-time
* Mobile apps
* Complex object hierarchy
* Complex queries

When to use REST:

* Non-interactive (system to syste)
* Microservices
* Simple object hierarchy
* Repeated, simple queries
