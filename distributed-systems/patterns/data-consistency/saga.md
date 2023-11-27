# Saga

A **saga** is a sequence of local transactions.

Each local transaction updates the database and publishes a message or event to trigger the next local transaction in the saga. If a local transaction fails because it violates a business rule then the saga executes a series of compensating transactions that undo the changes that were made by the preceding local transactions.

## Links

[↑ Pattern: Saga](https://microservices.io/patterns/data/saga.html).

[↑ Saga distributed transactions pattern](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/saga/saga).
