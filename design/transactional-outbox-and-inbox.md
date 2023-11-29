# Transactional outbox and inbox

## Outbox pattern

Outbox pattern idea: the message that you publish to the message broker or queue is being saved to the database a part of the same transaction as your state change. The library in the second process or thread will fetch data from the database the messages that haven't actually been publish yet and then it will send that to your message broker or your queue.

Why does it provide at-least-once and not exactly-once? Writing to the database may fail. When that happens, the process handling outbox pattern will try to resend the event after some time and try to do it until the message is correctly marked as sent in the database.

## Transactional inbox

**Transactional inbox** is a pattern that is similar to outbox pattern and is used to handle incoming messages from message broker or queue. Accordingly, we have a table in which we're storing incoming events. Contrary to outbox pattern, we first save the event in the database, then we're returning ACK to queue

## Links

[↑ Pattern: Transactional outbox](https://microservices.io/patterns/data/transactional-outbox.html).

[↑ Outbox pattern with Hibernate](https://thorben-janssen.com/outbox-pattern-hibernate/).

[↑ Transactional outbox pattern step by step with Spring and Kotlin](https://dev.to/aleksk1ng/transactional-outbox-pattern-step-by-step-with-spring-and-kotlin-3gkd).
