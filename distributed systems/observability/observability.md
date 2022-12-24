# Observability: logs, metrics, traces

**Observability** is a measure of how well we can understand the internal states of a system based on its external outputs. When you have *logs*, *metrics*, and *traces* you have the "3 pillars of observability".

## Logs

A **log** ia a timestamped record of an event or events.

A **log aggregation** is a practice of combining logs from many different services. It may give a snapshot of the activity within a collection of individual services, but the logs lack contextual metadata to provide the full picture of a request as it travels downstream through possibly millions of application dependencies. On its own, this method simply isn't sufficient for troubleshooting in distributed systems. This is where observability and, specifically, distributed tracing come in.

## Metrics

A **metric** is a numeric representation of data measured over a set period.

## Traces and spans

A **trace** ia a record of events that occur along the path of a single request.

A trace is a tree/list of *spans* representing the progression of requests as it is handled by the different services and components in a system. For example, sending an API call to user-service resulted in a DB query to users-db. They are "call-stacks" for distributed services.

A **span** is a representation of a unit of work (action/operation) that occurs in a system. Usually, it will be the parent and/or child of another span.

## Distributed tracing

A **distributed tracing** is a method of observing requests as they propagate through distributed cloud environments.

Distributed tracing follows an interaction by tagging it with a unique identifier. This identifier stays with the transaction as it interacts with microservices, containers, and infrastructure. The unique identifier offers real-time visibility into user experience, from the top of the stack to the application layer and the infrastructure beneath.

Distributed tracing helps teams understand more quickly how each microservice is performing.
