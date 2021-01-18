# Collections

## Immutable collections

**Immutable collections** are collection instances that can never change; since they cannot change, they are threadsafe.

Immutable collections are new, but they should be considered for new development unless you _need_ a mutable instance.

> Immutable collections are in the `System.Collections.Immutable` NuGet package.

There are several important design philosophies that are true for _all_ immutable collections:

- An instance of an immutable collection never changes.
- Since it never changes, it is naturally threadsafe.
- When you call a modifying method on an immutable collection, the new modified collection is returned.

| Immutable collection                                                          | Use case                                                                                                                                      |
| ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| ImmutableStack\<T>, ImmutableQueue\<T>                                        | You need a stack or queue that does not change very often and can be accessed by multiple threads safely.                                     |
| ImmutableList\<T>                                                             | You need a data structure you can index into that does not change very often and can be accessed by multiple threads safely.                  |
| ImmutableHashSet\<T>, ImmutableSortedSet\<T>                                  | You need a data structure that does not need to store duplicates, does not change very often, and can be accessed by multiple threads safely. |
| ImmutableDictionary\<TKey, TValue>, ImmutableSortedDictionary\<TKey, TValue>. | You need a key/value collection that does not change very often and can be accessed by multiple threads safely.                               |

## Threadsafe collections

**Threadsafe collections** are mutable collection instances can be changed by multiple threads simultaneously. Threadsafe collections use a mixture of fine-grained locks and lock-free techniques to ensure that threads are blocked for a minimal amount of time (and usually aren’t blocked at all). For many threadsafe collections, enumeration of the collection creates a snapshot of the collection and then enumerates that snapshot. The key advantage of threadsafe collections is that they can be accessed safely from multiple threads, yet the operations will only block your code for a short time, if at all.

| Threadsafe collection               | Use case                                                                                                                                                                                                                                                                          |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ConcurrentDictionary\<TKey, TValue> | You have a key/value collection (e.g., an in-memory cache) that you need to keep in sync, even though multiple threads are both reading from and writing to it.                                                                                                                   |
| BlockingCollection\<T>              | You need a conduit to pass messages or data from one thread to another. For example, one thread could be loading data, which it pushes down the conduit as it loads; meanwhile, there are other threads on the receiving end of the conduit that receive the data and process it. |
|                                     | You need a conduit to pass messages or data from one thread to another, but you don’t want (or need) the conduit to have first-in, first-out semantics.                                                                                                                           |

## Producer/consumer collections

**Producer/consumer collections** are collections that allow (possibly multiple) producers to push items to the collection while allowing (possibly multiple) consumers to pull items out of the collection. So they act as a bridge between producer code and consumer code, and they also have an option to limit the number of items in the collection. Producer/consumer collections can either have a blocking or asynchronous API.

> Channels can be found in the `System.Threading.Channels` NuGet package, `BufferBlock<T>` in the NuGet package for `System.Threading.Tasks.Dataflow`, and `syncProducerConsumerQueue<T>` and `AsyncCollection<T>` in the NuGet package for `Nito.AsyncEx`.
