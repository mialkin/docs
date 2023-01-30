# Transactional outbox and inbox

**Transactional outbox** is a pattern that ensures that a message was sent (e.g. to a queue) successfully at least once. With this pattern, instead of directly publishing a message to the queue, we store it in temporary storage (e.g. database table). We’re wrapping the entity save and message storing with the Unit of Work (transaction). By that, we’re ensuring that the message won’t be lost if the application data is stored. It will be published later through a background process. This process will check if there are any unsent messages in the table. When the worker finds any, it tries to send them. After it gets confirmation of publishing (e.g. ACK from the queue), it marks the event as sent.

Why does it provide at-least-once and not exactly-once? Writing to the database may fail (e.g. it will not respond). When that happens, the process handling outbox pattern will try to resend the event after some time and try to do it until the message is correctly marked as sent in the database.

**Transactional inbox** is a pattern that is similar to outbox pattern and is used to handle incoming messages (e.g. from a queue). Accordingly, we have a table in which we’re storing incoming events. Contrary to outbox pattern, we first save the event in the database, then we’re returning ACK to queue. If save succeeded, but we didn’t return ACK to queue, then delivery will be retried. That’s why we have at-least-once delivery again. After that, an outbox-like process runs. It calls message handlers that perform business logic.

[↑ Microservices 101: Transactional Outbox and Inbox](https://softwaremill.com/microservices-101/)

[↑ Pattern: Transactional outbox](https://microservices.io/patterns/data/transactional-outbox.html)