# Signaling, `AutoResetEvent`, `ManualResetEvent`, `ManualResetEventSlim`, `CountdownEvent`, `Barrier`

## Table of contents

- [Signaling, `AutoResetEvent`, `ManualResetEvent`, `ManualResetEventSlim`, `CountdownEvent`, `Barrier`](#signaling-autoresetevent-manualresetevent-manualreseteventslim-countdownevent-barrier)
  - [Table of contents](#table-of-contents)
  - [Signaling](#signaling)
  - [Comparison of signaling constructs](#comparison-of-signaling-constructs)
  - [Event wait handles](#event-wait-handles)
    - [`AutoResetEvent`](#autoresetevent)
    - [`ManualResetEvent`](#manualresetevent)
    - [`ManualResetEventSlim`](#manualreseteventslim)
    - [`CountdownEvent`](#countdownevent)
    - [`WaitAny`, `WaitAll`, and `SignalAndWait`](#waitany-waitall-and-signalandwait)
      - [`WaitAll`](#waitall)
      - [`WaitAny`](#waitany)
      - [`SignalAndWait`](#signalandwait)
      - [Alternatives to `WaitAll` and `SignalAndWait`](#alternatives-to-waitall-and-signalandwait)
    - [`EventWaitHandle`](#eventwaithandle)
    - [`WaitHandle`](#waithandle)
    - [Wait handles and the thread pool](#wait-handles-and-the-thread-pool)
  - [`Barrier`](#barrier)

## Signaling

A **signaling** is a mechanism that makes a thread to wait until it receives notification from another thread.

## Comparison of signaling constructs

Time taken to signal and wait on the construct once on the same thread, assuming no blocking, as measured on an [↑ Intel Core i7 860](https://www.intel.com/content/www/us/en/products/sku/41316/intel-core-i7860-processor-8m-cache-2-80-ghz/specifications.html):

| Construct                                       | Cross-process | Overhead             |
| ----------------------------------------------- | ------------- | -------------------- |
| [`AutoResetEvent`](#autoresetevent)             | Yes           | 1000 ns              |
| [`ManualResetEvent`](#manualresetevent)         | Yes           | 1000 ns              |
| [`ManualResetEventSlim`](#manualreseteventslim) | —             | 40 ns                |
| [`CountdownEvent`](#countdownevent)             | —             | 40 ns                |
| [`Barrier`](#barrier)                           | —             | 80 ns                |
| `Wait` and `Pulse`                              | —             | 120 ns for a `Pulse` |

## Event wait handles

An **event wait handle** is a construct used for signaling.

Event wait handles are the simplest of the signaling constructs, and they are unrelated to C# [events](/csharp/keywords/event.md). They come in three flavors: [`AutoResetEvent`](#autoresetevent), [`ManualResetEvent`](#manualresetevent), and [`CountdownEvent`](#countdownevent). The former two are based on the common [`EventWaitHandle`](#eventwaithandle) class, where they derive all their functionality.

### `AutoResetEvent`

The [↑ `AutoResetEvent`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.autoresetevent) class is a thread synchronization event that, when signaled, releases one single waiting thread and then resets automatically.

The `AutoResetEvent` is like a ticket turnstile: inserting a ticket lets exactly one person through. The "auto" in the class's name refers to the fact that an open turnstile automatically closes or "resets" after someone steps through. A thread waits, or blocks, at the turnstile by calling `WaitOne` (wait at this "one" turnstile until it opens), and a ticket is inserted by calling the `Set` method. If a number of threads call `WaitOne`, a queue builds up behind the turnstile. As with locks, the fairness of the queue can sometimes be violated due to nuances in the operating system. A ticket can come from any thread; in other words, any unblocked thread with access to the `AutoResetEvent` object can call `Set` on it to release one blocked thread.

You can create an `AutoResetEvent` in two ways. The first is via its constructor:

```csharp
var autoResetEvent = new AutoResetEvent(initialState: false);
```

Passing `true` into the constructor is equivalent to immediately calling `Set` upon it. The second way to create an `AutoResetEvent` is as follows:

```csharp
var autoResetEvent = new EventWaitHandle(initialState: false, EventResetMode.AutoReset);
```

In the following example, a thread is started whose job is simply to wait until signaled by another thread:

```csharp
var autoResetEvent = new AutoResetEvent(initialState: false);

var thread = new Thread(() =>
{
    Console.WriteLine("Start waiting");
    autoResetEvent.WaitOne();
    Console.WriteLine("End waiting");
});
thread.Start();

Thread.Sleep(5000);
Console.WriteLine("Setting the state of the event to signaled");
autoResetEvent.Set();

// Output:
// Start waiting
// Setting the state of the event to signaled
// End waiting
```

If `Set` is called when no thread is waiting, the handle stays open for as long as it takes until some thread calls `WaitOne`. This behavior helps avoid a race between a thread heading for the turnstile, and a thread inserting a ticket: "Oops, inserted the ticket a microsecond too soon, bad luck, now you'll have to wait indefinitely!". However, calling `Set` repeatedly on a turnstile at which no one is waiting doesn't allow a whole party through when they arrive: only the next single person is let through and the extra tickets are "wasted."

Calling `Reset` on an `AutoResetEvent` closes the turnstile, should it be open, without waiting or blocking.

`WaitOne` accepts an optional timeout parameter, returning `false` if the wait ended because of a timeout rather than obtaining the signal.

Calling `WaitOne` with a timeout of `0` tests whether a wait handle is "open," without blocking the caller. Bear in mind, though, that doing this resets the `AutoResetEvent` if it's open.

Once you've finished with a wait handle, you can call its `Close` method to release the operating system resource. Alternatively, you can simply drop all references to the wait handle and allow the garbage collector to do the job for you sometime later (wait handles implement the disposal pattern whereby the finalizer calls `Close`). This is one of the few scenarios where relying on this backup is (arguably) acceptable, because wait handles have a light OS burden.

Wait handles are released automatically when an application domain unloads.

### `ManualResetEvent`

The [↑ `ManualResetEvent`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.manualresetevent) class is a thread synchronization event that, when signaled, must be reset manually.

The `ManualResetEvent` functions like an ordinary gate. Calling `Set` opens the gate, allowing _any_ number of threads calling `WaitOne` to be let through. Calling `Reset` closes the gate. Threads that call `WaitOne` on a closed gate will block; when the gate is next opened, they will be released all at once. Apart from these differences, a `ManualResetEvent` functions like an [`AutoResetEvent`](#autoresetevent).

As with `AutoResetEvent`, you can construct a `ManualResetEvent` in two ways:

```csharp
var manualResetEvent = new ManualResetEvent(initialState: false);
var manualResetEvent2 = new EventWaitHandle(initialState: false, EventResetMode.ManualReset);
```

From Framework 4.0, there's another version of `ManualResetEvent` called [`ManualResetEventSlim`](#manualreseteventslim). The latter is optimized for short waiting times — with the ability to opt into spinning for a set number of iterations. It also has a more efficient managed implementation and allows a `Wait` to be canceled via a `CancellationToken`. It cannot, however, be used for interprocess signaling. `ManualResetEventSlim` doesn't subclass `WaitHandle`; however, it exposes a `WaitHandle` property that returns a `WaitHandle`-based object when called (with the performance profile of a traditional wait handle).

A `ManualResetEvent` is useful in allowing one thread to unblock many other threads. The reverse scenario is covered by [`CountdownEvent`](#countdownevent).

### `ManualResetEventSlim`

The [↑ `ManualResetEventSlim`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.manualreseteventslim) class is a thread synchronization event that, when signaled, must be reset manually. This class is a lightweight alternative to [`ManualResetEvent`](#manualresetevent).

### `CountdownEvent`

The [↑ `CountdownEvent`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.countdownevent) class is a synchronization primitive that is signaled when its count reaches zero.

`CountdownEvent` lets you wait on more than one thread.

To use `CountdownEvent`, instantiate the class with the number of threads or "counts" that you want to wait on:

```csharp
var countdownEvent = new CountdownEvent(initialCount: 3);
```

Calling `Signal` decrements the "count"; calling `Wait` blocks until the count goes down to zero:

```csharp
var countdownEvent = new CountdownEvent(initialCount: 4);

new Thread(() => Signal(5000)).Start();
new Thread(() =>
    {
        Signal(500);
        Signal(0);
        Console.WriteLine("Incrementing count by 1");
        countdownEvent.AddCount();
        Signal(1000);
    }
).Start();
new Thread(() => Signal(1000)).Start();

void Signal(int delay)
{
    Thread.Sleep(delay);
    Console.WriteLine($"Signaling from thread {Environment.CurrentManagedThreadId}. " +
                      $"Number of remaining signals required to set the event: {countdownEvent.CurrentCount}");
    countdownEvent.Signal();
}

Console.WriteLine("Waiting...");
countdownEvent.Wait();
Console.WriteLine("End");

// Output:
// Waiting...
// Signaling from thread 5. Number of remaining signals required to set the event: 4
// Signaling from thread 5. Number of remaining signals required to set the event: 3
// Incrementing count by 1
// Signaling from thread 6. Number of remaining signals required to set the event: 3
// Signaling from thread 5. Number of remaining signals required to set the event: 2
// Signaling from thread 4. Number of remaining signals required to set the event: 1
// End
```

Problems for which `CountdownEvent` is effective can sometimes be solved more easily using the _structured parallelism_ constructs (PLINQ and the `Parallel` class).

You can reincrement a `CountdownEvent`'s count by calling `AddCount`. However, if it has already reached zero, this throws an exception: you can't "unsignal" a `CountdownEvent` by calling `AddCount`. To avoid the possibility of an exception being thrown, you can instead call `TryAddCount`, which returns `false` if the countdown is zero.

To unsignal a countdown event, call `Reset`: this both unsignals the construct and resets its count to the original value.

Like `ManualResetEventSlim`, `CountdownEvent` exposes a [↑ `WaitHandle`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.waithandle) property for scenarios where some other class or method expects an object based on `WaitHandle`.

### `WaitAny`, `WaitAll`, and `SignalAndWait`

In addition to the `Set`, `WaitOne`, and `Reset` methods, there are static methods on the [`WaitHandle`](#waithandle) class to crack more complex synchronization nuts. The `WaitAny`, `WaitAll`, and `SignalAndWait` methods perform atomic signaling and waiting operations on multiple handles. The wait handles can be of differing types, including [`Mutex`](locking.md#mutex) and [`Semphore`](locking.md#semaphore-1), since these also derive from the abstract `WaitHandle` class. [`ManualResetEventSlim`](#manualreseteventslim) and [`CountdownEvent`](#countdownevent) can also partake in these methods via their `WaitHandle` properties.

#### `WaitAll`

```csharp
var autoResetEvent1 = new ManualResetEventSlim(initialState: false);
var autoResetEvent2 = new ManualResetEventSlim(initialState: false);

new Thread(() =>
{
    Thread.Sleep(5000);
    Console.WriteLine("Signaling 1");
    autoResetEvent1.Set();
}).Start();

new Thread(() =>
{
    Thread.Sleep(1000);
    Console.WriteLine("Signaling 2");
    autoResetEvent2.Set();
}).Start();

Console.WriteLine("Waiting all...");
WaitHandle.WaitAll([autoResetEvent1.WaitHandle, autoResetEvent2.WaitHandle]);
Console.WriteLine("End");

// Output:
// Waiting all...
// Signaling 2
// Signaling 1
// End
```

`WaitAll` and [`SignalAndWait`](#signalandwait) have a weird connection to the legacy COM architecture: these methods require that the caller be in a multithreaded apartment, the model least suitable for interoperability. The main thread of a WPF or Windows application, for example, is unable to interact with the clipboard in this mode.

#### `WaitAny`

By replacing `WaitAll` with `WaitAny` above you will get the following output:

```csharp
// Waiting all...
// Signaling 2
// End
// Signaling 1
```

#### `SignalAndWait`

`SignalAndWait` calls `Set` on one `WaitHandle`, and then calls `WaitOne` on another `WaitHandle`. The atomicity guarantee is that after signaling the first handle, it will jump to the head of the queue in waiting on the second handle: you can think of it as "swapping" one signal for another.

```csharp
var autoResetEvent1 = new ManualResetEventSlim(initialState: false);
var autoResetEvent2 = new ManualResetEventSlim(initialState: false);

new Thread(() =>
{
    autoResetEvent1.WaitHandle.WaitOne();
    Console.WriteLine("Got signaled");
}).Start();

new Thread(() =>
{
    Thread.Sleep(5000);
    Console.WriteLine("Signaling 2");
    autoResetEvent2.Set();
}).Start();

Thread.Sleep(1000);

Console.WriteLine("Signaling and waiting...");
WaitHandle.SignalAndWait(toSignal: autoResetEvent1.WaitHandle, toWaitOn: autoResetEvent2.WaitHandle);
Console.WriteLine("End");

// Output:
// Signaling and waiting...
// Got signaled
// Signaling 2
// End
```

You can use this method on a pair of `EventWaitHandles` to set up two threads to rendezvous or "meet" at the same point in time. Either `AutoResetEvent` or `ManualResetEvent` will do the trick. The first thread executes the following:

```csharp
WaitHandle.SignalAndWait(wh1, wh2);
```

whereas the second thread does the opposite:

```csharp
WaitHandle.SignalAndWait(wh2, wh1);
```

#### Alternatives to `WaitAll` and `SignalAndWait`

`WaitAll` and `SignalAndWait` won't run in a single-threaded apartment. Fortunately, there are alternatives. In the case of `SignalAndWait`, it's rare that you need its atomicity guarantee: in our rendezvous example, for instance, you could simply call `Set` on the first wait handle, and then `WaitOne` on the other. The [`Barrier`](#barrier) class provides yet another option for implementing a thread rendezvous.

In the case of `WaitAll`, an alternative in some situations is to use the `Parallel` class's `Invoke` method. The `Task.ContinueWhenAny` method provides an alternative to `WaitAny`.

In all other scenarios, the answer is to take the low-level approach that solves all signaling problems: `Wait` and `Pulse`.

### `EventWaitHandle`

The [↑ `EventWaitHandle`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.eventwaithandle) class represents a thread synchronization event.

`EventWaitHandle`'s constructor allows a "named" `EventWaitHandle` to be created, capable of operating across multiple processes. The name is simply a string, and it can be any value that doesn't unintentionally conflict with someone else's! If the name is already in use on the computer, you get a reference to the same underlying `EventWaitHandle`; otherwise, the operating system creates a new one:

```csharp
var eventWaitHandle = new EventWaitHandle (
    initialState: false, mode: EventResetMode.AutoReset, name: "MyCompany.MyApp.SomeName");
```

If two applications each ran this code, they would be able to signal each other: the wait handle would work across all threads in both processes.

### `WaitHandle`

The [↑ `WaitHandle`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.waithandle) encapsulates operating system-specific objects that wait for exclusive access to shared resources.

### Wait handles and the thread pool

If your application has lots of threads that spend most of their time blocked on a wait handle, you can reduce the resource burden by calling `ThreadPool.RegisterWaitForSingleObject.` This method accepts a delegate that is executed when a wait handle is signaled. While it's waiting, it doesn't tie up a thread:

```csharp
var manualResetEvent = new ManualResetEvent(initialState: false);

var registeredWaitHandle = ThreadPool.RegisterWaitForSingleObject(
    waitObject: manualResetEvent,
    callBack: Callback,
    state: new
    {
        Name = "Aleksei"
    },
    millisecondsTimeOutInterval: -1,
    executeOnlyOnce: true);

Thread.Sleep(5000);
Console.WriteLine("Signaling");
manualResetEvent.Set();

Console.ReadLine();

registeredWaitHandle.Unregister(waitObject: manualResetEvent);

void Callback(object? state, bool timedOut)
{
    Console.WriteLine(state);
}

// Output:
// Signaling
// { Name = Aleksei }
```

When the wait handle is signaled, or a timeout elapses, the delegate runs on a pooled thread.

## `Barrier`

The [↑ `Barrier`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.barrier) class enables multiple tasks to cooperatively work on an algorithm in parallel through multiple phases.

`Barrier` implements a _thread execution barrier_, which allows many threads to rendezvous at a point in time. The class is very fast and efficient, and is built upon `Wait`, `Pulse`, and spinlocks.

To use this class:

1. Instantiate it, specifying how many threads should partake in the rendezvous (you can change this later by calling `AddParticipants`/`RemoveParticipants`).
2. Have each thread call `SignalAndWait` when it wants to rendezvous.

Instantiating `Barrier` with a value of `3` causes `SignalAndWait` to block until that method has been called three times. But unlike a `CountdownEvent`, it then automatically starts over: calling `SignalAndWait` again blocks until called another three times. This allows you to keep several threads "in step" with each other as they process a series of tasks.

```csharp
var barrier = new Barrier(participantCount: 3, postPhaseAction: _ => Console.WriteLine());

new Thread(Speak).Start();
new Thread(Speak).Start();
new Thread(Speak).Start();

void Speak()
{
    for (var i = 1; i < 5; i++)
    {
        Console.Write(i + " ");
        barrier.SignalAndWait();
    }
}
// Output:
// 1 1 1
// 2 2 2
// 3 3 3
// 4 4 4
```

A really useful feature of `Barrier` is that you can also specify a _post-phase action_ when constructing it. This is a delegate that runs after `SignalAndWait` has been called _n_ times, but _before_ the threads are unblocked.

A post-phase action can be useful for coalescing data from each of the worker threads. It doesn't have to worry about preemption, because all workers are blocked while it does its thing.
