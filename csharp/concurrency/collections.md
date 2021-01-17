# Collections

## Immutable collections

**Immutable collections** are collection instances that can never change; since they cannot change, they are threadsafe.

Immutable collections are new, but they should be considered for new development unless you *need* a mutable instance.

> Immutable collections are in the `System.Collections.Immutable` NuGet package.

## Threadsafe collections

**Threadsafe collections** are mutable collection instances can be changed by multiple threads simultaneously. Threadsafe collections use a mixture of fine-grained locks and lock-free techniques to ensure that threads are blocked for a minimal amount of time (and usually aren’t blocked at all). For many threadsafe collections, enumeration of the collection creates a snapshot of the collection and then enumerates that snapshot. The key advantage of threadsafe collections is that they can be accessed safely from multiple threads, yet the operations will only block your code for a short time, if at all.

## Producer/consumer collections

**Producer/consumer collections** are collections that allow (possibly multiple) producers to push items to the collection while allowing (possibly multiple) consumers to pull items out of the collection. So they act as a bridge between producer code and consumer code, and they also have an option to limit the number of items in the collection. Producer/consumer collections can either have a blocking or asynchronous API.

> Channels can be found in the `System.Threading.Channels` NuGet package, `BufferBlock<T>` in the NuGet package for `System.Threading.Tasks.Dataflow`, and `syncProducerConsumerQueue<T>` and `AsyncCollection<T>` in the NuGet package for `Nito.AsyncEx`.
