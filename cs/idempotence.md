# Idempotence

**Idempotence** is the property that an operation may be applied multiple times with the result not differing from the first application. Restated, that means multiple identical requests should have the same effect as a single request.

> A typical crosswalk button is an example of an idempotent system

From a RESTful service standpoint, for an operation to be idempotent, clients can make that same call repeatedly while producing the same result. In other words, making multiple identical requests has the same effect as making a single request.
