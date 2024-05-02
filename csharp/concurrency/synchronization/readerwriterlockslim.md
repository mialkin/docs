# `ReaderWriterLockSlim`

Represents a lock that is used to manage access to a resource, allowing multiple threads for reading or exclusive access for writing.

Quite often, instances of a type are thread-safe for concurrent read operations, but not for concurrent updates (nor for a concurrent read and update). This can also be true with resources such as a file. Although protecting instances of such types with a simple exclusive lock for all modes of access usually does the trick, it can unreasonably restrict concurrency if there are many readers and just occasional updates. An example of where this could occur is in a business application server, where commonly used data is cached for fast retrieval in static fields. The `ReaderWriterLockSlim` class is designed to provide maximum-availability locking in just this scenario.

> `ReaderWriterLockSlim` was introduced in Framework 3.5 and is a replacement for the older "fat" `ReaderWriterLock` class. The latter is similar in functionality, but it is several times slower and has an inherent design fault in its mechanism for handling lock upgrades.

When compared to an ordinary lock (`Monitor.Enter`/`Exit`), `ReaderWriterLockSlim` is twice as slow.

## Links

[↑ Microsoft documentation — ReaderWriterLockSlim Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.readerwriterlockslim)
