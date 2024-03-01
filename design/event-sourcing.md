# Event sourcing

**Event sourcing** is a pattern for tracking changes to application state, stored as a sequence of events.

Event sourcing puts a greater emphasis on how data evolves over time. It captures the journey, rather than just the destination. This allows systems based on event sourcing to have access to richer data that can be used for analytical purposes, but also to help develop new features.

## Table of contents

- [Event sourcing](#event-sourcing)
  - [Table of contents](#table-of-contents)
  - [Snapshots](#snapshots)
  - [Benefits of event sourcing](#benefits-of-event-sourcing)
    - [Audit trail](#audit-trail)
    - [Historical queries](#historical-queries)
    - [Future-proofing](#future-proofing)
    - [Performance and scalability](#performance-and-scalability)
    - [Resilience](#resilience)
  - [Links](#links)

## Snapshots

Over time, as the log of events grows, it may become inefficient to replay them all. In this case, event-sourced systems often leverage **snapshots**. These snapshots record the current state of the system at the time the snapshot is created.

When using snapshots, the system doesn’t need to replay all events. Instead, it can restore the most recent snapshot and only replay events that occurred after the snapshot was taken.

Once snapshots are introduced, this starts to look like a more traditional state based system. The traditional system was the equivalent of doing a snapshot on every write. However, those systems didn't keep the event log and discarded any old snapshots.

With event sourcing, although old snapshots can be discarded, they don't have to be. They can be kept if they are valuable. Furthermore, the snapshots are simply an optimization. They shouldn't be created on every write, and at any point they can be thrown away because they can always be reconstructed from the events.

## Benefits of event sourcing

### Audit trail

Every action taken is stored as an event which creates a natural audit trail for compliance and security. It should be possible to analyze the events and determine exactly how the current state was reached.

### Historical queries

Events can be used to execute historical queries. Rather than looking only at the state as it currently exists, it can be replayed up to a specific time. This allows the exact state to be reconstructed at any point in the past.

### Future-proofing

Businesses are always evolving and coming up with new ways to use data. The data that is thrown away today might be critical in the future. Because event-sourced systems never throw away data, they enable future developers to build features than nobody could have predicted would be necessary.

### Performance and scalability

Event-sourced systems operate in an append-only fashion. This has implications for how the database is used because it eliminates the need for some types of database locks. These locks create contention which decreases scalability and can impact performance. By reducing the locks it improves the system's ability to perform at scale.

### Resilience

What happens to a traditional system when a bug is introduced that miscalculates the current state? The old state is lost and the new state is corrupted. However, an event-sourced system doesn’t record the state. Instead, it records the intent. By capturing the intent we have the potential to fix problems that would otherwise be impossible. State can be restored to earlier points to clear corruption and once a bug is fixed, the events can be replayed to calculate the correct state.

## Links

[↑ What is the Event Sourcing Pattern?](https://www.youtube.com/watch?v=wPwD9CQAGsk).

[↑ What is event sourcing?](https://www.confluent.io/learn/event-sourcing).
