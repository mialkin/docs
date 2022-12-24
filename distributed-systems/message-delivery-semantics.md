# Message delivery semantics

## At most once

**At most once** — messages may be lost but are never redelivered.

In an "at most once" delivery semantics, a message should be delivered maximum only once. It's acceptable to lose a message rather than delivering a message twice in this semantic. A few use cases of at most once includes metrics collection, log collection, and so on. Applications adopting at most semantics can easily achieve higher throughput and low latency.

## At least once

**At least once** – messages are never lost but may be redelivered.

In "at least once" delivery semantics it is acceptable to deliver a message more than once but no message should be lost. The producer ensures that all messages are delivered for sure, even though it may result in message duplication. This is the most preferred semantics system out of them all. Applications adopting at least once semantics may have moderate throughput and moderate latency.

## Exactly once

**Exactly once** – this is what people actually want, each message is delivered once and only once.

In 'exactly one' delivery semantics, a message must be delivered only once and no message should be lost. This is the most difficult delivery semantic of all. Applications adopting exactly once semantics may have lower throughput and higher latency than the other two semantic systems we've looked at.
