# `Monitor`

`Monitor` locks objects (that is, reference types), not value types. While you can pass a value type to `Enter` and `Exit`, it is boxed separately for each call. Since each call creates a separate object, `Enter` never blocks, and the code it is supposedly protecting is not really synchronized. In addition, the object passed to `Exit` is different from the object passed to `Enter`, so `Monitor` throws `SynchronizationLockException` exception with the message "Object synchronization method was called from an unsynchronized block of code."

Use the `Monitor` class to lock objects other than strings (that is, reference types other than `String`), not value types.

## The critical section

Use the `Enter` and `Exit` methods to mark the beginning and end of a critical section.

> The functionality provided by the `Enter` and `Exit` methods is identical to that provided by the `lock` statement in C#, except that the language constructs wrap the `Monitor.Enter(Object, Boolean)` method overload and the `Monitor.Exit` method in a `try`…`finally` block to ensure that the monitor is released.

## `Monitor` vs `Mutex`

A `Monitor` is managed, and more lightweight — but is restricted to your `AppDomain`. A `Mutex` can be named, and can span processes (allowing some simple IPC scenarios between applications), and can be used in code that wants a wait-handle).

For most simple scenarios, `Monitor` (via `lock`) is fine.

## Links

[↑ Monitor Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.monitor)
