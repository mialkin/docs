# ACID

**ACID** (atomicity, consistency, isolation, durability) is a set of properties of database transactions intended to guarantee data validity despite errors, power failures, and other mishaps.

## Atomicity

**Atomicity** is a property that guarantees that each transaction is treated as a single "unit", which either succeeds completely, or fails completely: if any of the statements constituting a transaction fails to complete, the entire transaction fails and the database is left unchanged.

## Consistency

**Consistency** is a property that guarantees that a transaction can only bring the database from one valid state to another, maintaining database invariants: any data written to the database must be valid according to all defined rules, including constraints, cascades, triggers, and any combination thereof.

## Isolation

**Isolation**  is a property that guarantees that concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially.

## Durability

**Durability** is a property that  guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure (e.g., power outage or system crash). This usually means that completed transactions are recorded in non-volatile memory.
