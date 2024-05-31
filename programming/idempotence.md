# Idempotence

An **idempotence** is the property that an operation may be applied multiple times with the result not differing from the first application.

A typical crosswalk button is an example of an idempotent system.

In RabbitMQ declaring a queue is idempotent — it will only be created if it doesn't exist already.

## RESTful services

From a RESTful service standpoint, for an operation to be idempotent, clients can make that same call repeatedly while producing the same result. In other words, making multiple identical requests has the same effect as making a single request.

## HTTP methods

Idempotence is an important property of some HTTP methods, ensuring that executing the same request multiple times produces the same result as if it was performed only once.

GET, HEAD, PUT, DELETE, OPTIONS, and TRACE are idempotent methods, meaning they are safe to be retried or executed multiple times without causing unintended side effects.

In contrast, POST and PATCH are generally considered non-idempotent, as their outcomes may vary with each request.
