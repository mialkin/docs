# Synchronization

- [Synchronization](#synchronization)
  - [Using `lock` statement](#using-lock-statement)
  - [Additional stuff](#additional-stuff)
  - [Immutable collections](#immutable-collections)
  - [Threadsafe collections](#threadsafe-collections)

When your application makes use of concurrency (as practically all .NET applications do), then you need to watch out for situations in which one piece of code needs to update data while other code needs to access the same data. Whenever this happens, you need to _synchronize_ access to the
data.

There are two major types of synchronization: _communication_ and _data protection_.

You need to use synchronization to protect shared data when all three of these conditions are true:

- Multiple pieces of code are running concurrently.
- These pieces are accessing (reading or writing) the same data.
- At least one piece of code is updating (writing) the data.

In other words, only data that is both _shared_ and _updated_ needs synchronization.

## Using `lock` statement

There are many other kinds of locks in the .NET framework except `lock` statement, such as `Monitor`, `SpinLock`, and `ReaderWriterLockSlim`. In most applications, these lock types should almost never be used directly. In particular, it’s natural for developers to jump to `ReaderWriterLockSlim` when there is no need for that level of complexity. The basic lock statement handles 99% of cases quite well.

There are four important guidelines when using locks:

- Restrict lock visibility.
- Document what the lock protects.
- Minimize code under lock.
- Never execute arbitrary code while holding a lock.

## Additional stuff

Does this code need synchronization? :

```csharp
async Task MyMethodAsync()
{
    int value = 10;
    await Task.Delay(TimeSpan.FromSeconds(1));

    value = value + 1;
    await Task.Delay(TimeSpan.FromSeconds(1));

    value = value - 1;
    await Task.Delay(TimeSpan.FromSeconds(1));

    Trace.WriteLine(value);
}
```

The anser is NO: each thread has it's own value of `value` in its stack and the method is asynchronous, but it's also sequential. Each thread has its own independent stack but shares the same memory with all the other threads in a process.

Does this code need synchronization? :

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

int result = await new A().ModifyValueConcurrentlyAsync();
Console.WriteLine(result);

class A
{
    private int value;

    async Task ModifyValueAsync()
    {
        Console.WriteLine("Before ThreadId: " + Thread.CurrentThread.ManagedThreadId);
        await Task.Delay(TimeSpan.FromSeconds(5));
        Console.WriteLine("After ThreadId: " + Thread.CurrentThread.ManagedThreadId);
        value = value + 1;
    }

    // WARNING: may require synchronization; see discussion below.
    public async Task<int> ModifyValueConcurrentlyAsync()
    {
        // Start three concurrent modifications.
        Task task1 = ModifyValueAsync();
        Task task2 = ModifyValueAsync();
        Task task3 = ModifyValueAsync();

        await Task.WhenAll(task1, task2, task3);

        return value;
    }
}
```

The answer is: it tepends. If you know that the method is called from a GUI or ASP.NET context (or any context that only allows one piece of code to run at a time), synchronization won’t be necessary because when the actual `data` modification code runs, it runs at a different time than the other two `data` modifications. For example, if the preceding code is run in a GUI context, there’s only one UI thread that will execute each of the `data` modifications, so it _must_ do them one at a time. So, if you know the context is a one-at-a-time context, then there’s no synchronization needed. However, if that same method is called from a threadpool thread (e.g., from `Task.Run`), then synchronization _would_ be necessary. In that case, the three `data` modifications could run on separate threadpool threads and update `data.Value` simultaneously, so you would need to synchronize access to `data.Value`.

Result for .NET 5 console application:

```terminal
Before ThreadId: 1
Before ThreadId: 1
Before ThreadId: 1
After ThreadId: 7
After ThreadId: 8
After ThreadId: 9
3
```

Result for .NET 5 WPF application:

```terminal
Before ThreadId: 1
Before ThreadId: 1
Before ThreadId: 1
After ThreadId: 1
After ThreadId: 1
After ThreadId: 1
3
```

In both cases the execution times stays the same — 5 seconds.

## Immutable collections

Immutable types are naturally threadsafe because they cannot change; it’s not possible to update an immutable collection, so no synchronization is necessary.

## Threadsafe collections

Threadsafe collections (e.g. `ConcurrentDictionary`) are quite different. Unlike immutable collections, threadsafe collections can be updated. But they have all the synchronization they need built in, so you don’t have to worry about synchronizing collection changes.
