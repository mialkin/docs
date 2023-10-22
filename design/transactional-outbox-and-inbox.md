# Transactional outbox and inbox

## Outbox pattern

Outbox pattern idea: the message that you publish to the message broker or queue is being saved to the database a part of the same transaction as your state change. The library in the second process or thread will fetch data from the database the messages that haven't actually been publish yet and then it will send that to your message broker or your queue.

Why does it provide at-least-once and not exactly-once? Writing to the database may fail. When that happens, the process handling outbox pattern will try to resend the event after some time and try to do it until the message is correctly marked as sent in the database.

## Transactional inbox

**Transactional inbox** is a pattern that is similar to outbox pattern and is used to handle incoming messages from message broker or queue. Accordingly, we have a table in which we're storing incoming events. Contrary to outbox pattern, we first save the event in the database, then we're returning ACK to queue

## Links

[↑ How to Deliver Event Messages Successfully | Outbox Pattern](https://www.youtube.com/watch?v=vM-WNyEiNk8).

[↑ Outbox, Inbox patterns and delivery guarantees explained](https://event-driven.io/en/outbox_inbox_patterns_and_delivery_guarantees_explained).

[↑ Inbox Pattern & Duplicate detection](https://awesome-architecture.com/cloud-design-patterns/inbox-pattern).

[↑ Microservices 101: Transactional Outbox and Inbox](https://softwaremill.com/microservices-101).

[↑ Pattern: Transactional outbox](https://microservices.io/patterns/data/transactional-outbox.html).
