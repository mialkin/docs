# Database per service

**Database per service** is an application architecture pattern that keeps each microservice's persistent data private to that service and accessible only via its API. A service's transactions only involve its database.

The service's database is effectively part of the implementation of that service. It cannot be accessed directly by other services.

[↑ Pattern: Database per service](https://microservices.io/patterns/data/database-per-service.html).

## Table of contents

- [Database per service](#database-per-service)
  - [Table of contents](#table-of-contents)
  - [Implementation](#implementation)
  - [Advantages](#advantages)
  - [Disadvantages](#disadvantages)
  - [Implementing transactions and queries](#implementing-transactions-and-queries)

## Implementation

There are a few different ways to keep a service's persistent data private. You do not need to provision a database server for each service. For example, if you are using a relational database then the options are:

- Private tables per service – each service owns a set of tables that must only be accessed by that service
- Schema per service – each service has a database schema that's private to that service
- Database server per service – each service has it's own database server.

Private tables per service and schema per service have the lowest overhead. Using a schema per service is appealing since it makes ownership clearer. Some high throughput services might need their own database server.

It is a good idea to create barriers that enforce this modularity. You could, for example, assign a different database user ID to each service and use a database access control mechanism such as grants. Without some kind of barrier to enforce encapsulation, developers will always be tempted to bypass a service's API and access it's data directly.

## Advantages

- Helps ensure that the services are loosely coupled. Changes to one service's database does not impact any other services.
- Each service can use the type of database that is best suited to its needs. For example, a service that does text searches could use ElasticSearch. A service that manipulates a social graph could use Neo4j.

## Disadvantages

- Implementing business transactions that span multiple services is not straightforward. Distributed transactions are best avoided because of the CAP theorem. Moreover, many modern (NoSQL) databases don't support them.
- Implementing queries that join data that is now in multiple databases is challenging.
- Complexity of managing multiple SQL and NoSQL databases

## Implementing transactions and queries

There are various patterns/solutions for implementing transactions and queries that span services:

- Implementing transactions that span services — use Saga pattern.
- Implementing queries that span services:
  - API composition — the application performs the join rather than the database. For example, a service, or the API gateway, could retrieve a customer and their orders by first retrieving the customer from the customer service and then querying the order service to return the customer’s most recent orders.
  - CQRS — maintain one or more materialized views that contain data from multiple services. The views are kept by services that subscribe to events that each services publishes when it updates its data. For example, the online store could implement a query that finds customers in a particular region and their recent orders by maintaining a view that joins customers and orders. The view is updated by a service that subscribes to customer and order events.
