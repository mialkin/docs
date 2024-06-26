# Transactional outbox, transactional inbox, dual-write problem

## Transactional outbox

The **transactional outbox** is a technique used to overcome the [dual-write problem](#dual-write-problem).

If we have a database that supports transactional updates, then we can use it to overcome the dual-write problem. Rather than trying to update two separate systems at the same time, we push the transactional logic into *outbox table* in the database. We can use a separate background job/process that asynchronously monitors the outbox table. Whenever it sees a new record it starts processing. Once the processing has been finished successfully, the record can be removed from the outbox table, or marked as processed. If processing fails, then transaction rolls back and background job/process will try to process the record again.

Transactional outbox guarantees at least once processing. We need to make sure that downstream systems are prepared to handle any duplicates.

## Transactional inbox

The **transactional inbox** is a mechanism that guarantees idempotent event processing within a single database that supports transactional update.

When a new event arrives you open up a database transaction and insert unique ID of this event into the inbox table. In the same transaction you update other tables in the database.

Transactional inbox guarantees that possible multiple events with the same ID will be processed at most once.

Column that stores ID must have `UNIQUE` constraint.

## Dual-write problem

The **dual-write problem** is a challenge which occurs when you have to write data to two separate systems. Because these two systems aren't linked we have no way to update both in a transactional fashion.

There are valid options for avoiding the dual write problem, such as a [transactional outbox](#transactional-outbox), [event sourcing](/design/event-sourcing.md), and [↑ change data capture](https://en.wikipedia.org/wiki/Change_data_capture).

[↑ What is the Dual Write Problem?](https://www.youtube.com/watch?v=FpLXCBr7ucA).
