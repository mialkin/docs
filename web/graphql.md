# GraphQL

**GraphQL** is a query and a manipulation language for APIs and a runtime for fulfilling those queries with your existing data.

GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.

## Core concepts

* **Schema** is what describes the API in full. It's self-documenting and is comprised of *types*. It must have a *root query type*.
* **Types** can be anything in GraphQL: *query*, *mutation*, *subscription*, *object*, *enumeration*, *scalar* (id, int, string, boolean, float).
* **Resolvers** is a way to get a data for a given field. They can resolve to anything (e.g. database, microservice, REST API).

## GraphQL vs REST

There is *nothing wrong* with REST, but:

* REST over-fetches: returning more data than you need