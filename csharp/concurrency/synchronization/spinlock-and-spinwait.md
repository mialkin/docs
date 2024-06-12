# `SpinLock` and `SpinWait`

- [`SpinLock` and `SpinWait`](#spinlock-and-spinwait)
  - [`SpinLock`](#spinlock)
  - [`SpinWait`](#spinwait)
    - [How SpinWait works](#how-spinwait-works)
  - [Links](#links)

In parallel programming, a brief episode of spinning is often preferable to blocking, as it avoids the cost of context switching and kernel transitions. `SpinLock` and `SpinWait` are designed to help in such cases. Their main use is in writing custom synchronization constructs.

> `SpinLock` and `SpinWait` are structs and not classes! This design decision was an extreme optimization technique to avoid the cost of indirection and garbage collection. It means that you must be careful not to unintentionally _copy_ instances—by passing them to another method without the `ref` modifier, for instance, or declaring them as `readonly` fields. This is particularly important in the case of `SpinLock`.

## `SpinLock`

The `SpinLock` struct lets you lock without incurring the cost of a context switch, at the expense of keeping a thread spinning (uselessly busy). This approach is valid in high-contention scenarios when locking will be very brief (e.g., in writing a thread-safe linked list from scratch).

> If you leave a spinlock contended for too long (we're talking milliseconds at most), it will yield its time slice, causing a context switch just like an ordinary lock. When rescheduled, it will yield again—in a continual cycle of "spin yielding." This consumes far fewer CPU resources than outright spinning—but more than blocking. On a single-core machine, a spinlock will start "spin yielding" immediately if contended.

Using a `SpinLock` is like using an ordinary `lock`, except:

- Spinlocks are structs (as previously mentioned).
- Spinlocks are not reentrant, meaning that you cannot call `Enter` on the same `SpinLock` twice in a row on the same thread. If you violate this rule, it will either throw an exception (if _owner tracking_ is enabled) or deadlock (if owner tracking is disabled). You can specify whether to enable owner tracking when constructing the spinlock. Owner tracking incurs a performance hit.
- `SpinLock` lets you query whether the lock is taken, via the properties `IsHeld` and, if owner tracking is enabled, `IsHeldByCurrentThread`.
- There's no equivalent to C#'s `lock` statement to provide `SpinLock` syntactic sugar.

Another difference is that when you call `Enter`, you _must_ follow the robust pattern of providing a `lockTaken` argument (which is nearly always done within a `try/finally` block).

Here's an example:

```csharp
var spinLock = new SpinLock (true); // Enable owner tracking
bool lockTaken = false;
try
{
    spinLock.Enter (ref lockTaken);
    // Do stuff...
}
finally
{
    if (lockTaken) spinLock.Exit();
}
```

As with an ordinary `lock`, `lockTaken` will be false after calling `Enter` if (and only if) the `Enter` method throws an exception and the `lock` was not taken. This happens in very rare scenarios (such as `Abort` being called on the thread, or an `OutOfMemoryException` being thrown) and lets you reliably know whether to subsequently call `Exit`.

`SpinLock` also provides a `TryEnter` method which accepts a timeout.

> Given `SpinLock`'s ungainly value-type semantics and lack of language support, it's almost as if they _want_ you to suffer every time you use it! Think carefully before dismissing an ordinary `lock`.

A `SpinLock` makes the most sense when writing your own reusable synchronization constructs. Even then, a spinlock is not as useful as it sounds. It still limits concurrency. And it wastes CPU time doing _nothing useful_. Often, a better choice is to spend some of that time doing something _speculative_ — with the help of `SpinWait`.

> Spinlocks are only useful in places where anticipated waiting time is shorter than a quantum (read: milliseconds) and preemption doesn't make much sense (e.g. kernel objects aren't available).<sup>1</sup>

## `SpinWait`

`SpinWait` helps you write lock-free code that spins rather than blocks. It works by implementing safeguards to avoid the dangers of resource starvation and priority inversion that might otherwise arise with spinning.

> Lock-free programming with `SpinWait` is as _hardcore_ as multithreading gets and is intended for when none of the higher-level constructs will do. A prerequisite is to understand "Nonblocking Synchronization".

### How SpinWait works

In its current implementation, `SpinWait` performs CPU-intensive spinning for 10 iterations before yielding. However, it doesn't return to the caller _immediately_ after each of those cycles: instead, it calls `Thread.SpinWait` to spin via the CLR (and ultimately the operating system) for a set time period. This time period is initially a few tens of nanoseconds, but doubles with each iteration until the 10 iterations are up. This ensures some predictability in the total time spent in the CPU-intensive spinning phase, which the CLR and operating system can tune according to conditions. Typically, it's in the few-tens-of-microseconds region—small, but more than the cost of a context switch.

On a single-core machine, `SpinWait` yields on every iteration. You can test whether `SpinWait` will yield on the next spin via the property `NextSpinWillYield`.

If a `SpinWait` remains in "spin-yielding" mode for long enough (maybe 20 cycles) it will periodically sleep for a few milliseconds to further save resources and help other threads progress.

`SpinWait` is a value type, which means that low-level code can utilize `SpinWait` without fear of unnecessary allocation overheads. `SpinWait` is not generally useful for ordinary applications. In most cases, you should use the synchronization classes provided by the .NET Framework, such as `Monitor`. For most purposes where spin waiting is required, however, the `SpinWait` type should be preferred over the `Thread.SpinWait` method.<sup>2</sup>

## Links

[↑ SpinLock and SpinWait](http://www.albahari.com/threading/part5.aspx#_SpinLock_and_SpinWait)

<sup>1</sup> [↑ Stack Overflow — Does that mean I should use spinlocks wherever possible?](https://stackoverflow.com/a/1957464/1833895)

<sup>2</sup> [↑ Microsoft documentation — SpinWait Struct](https://docs.microsoft.com/en-us/dotnet/api/system.threading.spinwait#remarks)
