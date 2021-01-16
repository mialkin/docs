# Synchronization

There are two major types of synchronization: *communication* and *data protection*.

## Using synchronization for data protection

You need to use synchronization to protect shared data when all three of these conditions are true:

* Multiple pieces of code are running concurrently.
* These pieces are accessing (reading or writing) the same data.
* At least one piece of code is updating (writing) the data.

### Immutable collections

Immutable types are naturally threadsafe because they cannot change; it’s not possible to update an immutable collection, so no synchronization is necessary.

### Threadsafe collections

Threadsafe collections (e.g., `ConcurrentDictionary`) are quite different. Unlike immutable collections, threadsafe collections can be updated. But they have all the synchronization they need built in, so you don’t have to worry about synchronizing collection changes.
