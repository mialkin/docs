# Optimistic and pessimistic concurrency

## Optimistic concurrency

The optimistic concurrency feature provides the ability for your applications to detect whether a database table record has changed on the server in the time between when your application retrieved the record and when it tries to update or delete that record.

You can easily implement optimistic concurrency in any database by using a timestamp or a version number. Then, when a user is trying to commit a change to the database, you can compare the version number or the timestamp and decide which record to keep.

Users who use optimistic concurrency do not lock a database row when reading it. When a user wants to update a row, the application must determine whether another user has changed the row since it was read.

Optimistic concurrency is generally used in environments with a low contention for data. Optimistic concurrency improves performance because no locking of records is required, and locking of records requires additional server resources. Also, in order to maintain record locks, a persistent connection to the database server is required. Because this is not the case in an optimistic concurrency model, connections to the server are free to serve a larger number of clients in less time.

In an optimistic concurrency model, a violation is considered to have occurred if, after a user receives a value from the database, another user modifies the value before the first user has attempted to modify it. The concurrency violation simply lets you know that the update failed.

Advantages of optimistic concurrency:

- Does not require locks
- Offers support to scale applications
- Can serve multiple users at once, i.e. scales
- No deadlock situations
- Does not affect performance

Disadvantages of optimistic concurrency:

- Requires maintenance of versions or timestamps
- Requires manual implementation of concurrency handling logic

[↑ Optimistic Concurrency - ADO.NET](https://learn.microsoft.com/en-us/dotnet/framework/data/adonet/optimistic-concurrency).

## Pessimistic concurrency

Pessimistic concurrency involves locking rows at the data source to prevent other users from modifying data in a way that affects the current user. In a pessimistic model, when a user performs an action that causes a lock to be applied, other users cannot perform actions that would conflict with the lock until the lock owner releases it. This model is primarily used in environments where there is heavy contention for data, so that the cost of protecting data with locks is less than the cost of rolling back transactions if concurrency conflicts occur.

In a pessimistic concurrency model, a user who updates a row establishes a lock. Until the user has finished the update and released the lock, no one else can change that row. For this reason, pessimistic concurrency is best implemented when lock times will be short, as in programmatic processing of records. Pessimistic concurrency is not a scalable option when users are interacting with data and causing records to be locked for relatively large periods of time.

Advantages of pessimistic concurrency:

- Built-in database support
- Prevents conflicts from the moment the transaction starts

Disadvantages of pessimistic concurrency:

- There can be performance issues if the lock duration is high
- Deadlock situations can happen
- Affects application scalability
- Is not supported by all the databases
- High resource consumption due to locking and waiting

[↑ Optimistic vs Pessimistic Concurrency: What Every Developer Should Know](https://cult.honeypot.io/reads/optimistic-vs-pessimistic-concurrency).
