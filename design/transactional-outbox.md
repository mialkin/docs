# Transactional outbox

The **transactional outbox** is a technique used to overcome the *dual-write problem*.

The **dual-write problem** is a challenge which occurs when you have to write data to two separate systems.

Transactional outbox guarantees at least once guarantee of asynchronous processing.

A separate background job/process is used for outbox table processing. If your business logic fails you rollback transaction and try again.
