# Dual write problem

The **dual write problem** occurs when your service needs to write to two external systems in an atomic fashion. A common example would be writing state to a database and publishing an event to Apache Kafka. The separate systems prevent you from using a transaction, and as a result, if one write fails it can leave the other in an inconsistent state. This is an easy trap to fall into.

There are valid options for avoiding the dual write problem, such as a [transactional outbox](/design/transactional-outbox.md), [event sourcing](/design/event-sourcing.md), and [↑ change data capture](https://en.wikipedia.org/wiki/Change_data_capture). However, we have to be careful to avoid solutions that seem valid on the surface but just move the problem.

## Links

[↑ What is the Dual Write Problem?](https://www.youtube.com/watch?v=FpLXCBr7ucA).
