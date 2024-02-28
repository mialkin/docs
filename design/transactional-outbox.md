# Transactional outbox

**Transactional outbox** is a pattern that leverages database transactions to update a microservice's state and an outbox table.

This technique is used to overcome the dual-write problem which occurs when you have to write data to two separate systems such as a database and Apache Kafka. The database transactions can be used to ensure atomic writes between the two tables. From there, a separate process can consume the outbox and update the external system as required. This process can be written manually, or we can leverage tools such as [↑ Change Data Capture, CDC](https://www.confluent.io/learn/change-data-capture/) or Kafka connectors.

## Links

[↑ What is the Transactional Outbox Pattern?](https://www.youtube.com/watch?v=5YLpjPmsPCA).
