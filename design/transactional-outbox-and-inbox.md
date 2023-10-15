# Transactional outbox and inbox

## Outbox pattern

Outbox pattern idea: the message that you publish to the message broker or queue is being saved to the database a part of the same transaction as your state change. The library in the second process or thread will fetch data from the database the messages that haven't actually been publish yet and then it will send that to your message broker or your queue.

asdf

## Links

[↑ Outbox, Inbox patterns and delivery guarantees explained](https://event-driven.io/en/outbox_inbox_patterns_and_delivery_guarantees_explained).

[↑ Inbox Pattern & Duplicate detection](https://awesome-architecture.com/cloud-design-patterns/inbox-pattern).

[↑ Microservices 101: Transactional Outbox and Inbox](https://softwaremill.com/microservices-101).

[↑ Pattern: Transactional outbox](https://microservices.io/patterns/data/transactional-outbox.html).
