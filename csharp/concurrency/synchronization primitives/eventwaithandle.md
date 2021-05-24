# `EventWaitHandle`, `AutoResetEvent`, `ManualResetEvent`, `ManualResetEventSlim`

The `EventWaitHandle` class represents a thread synchronization event.

Event wait handles are used for signaling. **Signaling** is when one thread waits until it receives notification from another.

Event wait handles are the simplest of the signaling constructs, and they are unrelated to C# events.

A synchronization event can be either in an *unsignaled* or *signaled* state. When the state of an event is unsignaled, a thread that calls the event's `WaitOne` overload is blocked until an event is signaled. The `EventWaitHandle.Set` method sets the state of an event to signaled.

The behavior of an `EventWaitHandle` that has been signaled depends on its reset mode:

- An `EventWaitHandle` created with the `EventResetMode.AutoReset` flag resets automatically after releasing a single waiting thread. It's like a turnstile that allows only one thread through each time it's signaled. `AutoResetEvent` class, which derives from `EventWaitHandle`, represents that behavior.
- An `EventWaitHandle` created with the `EventResetMode.ManualReset` flag remains signaled until its `Reset` method is called. It's like a gate that is closed until signaled and then stays open until someone closes it. The `ManualResetEvent` class, which derives from `EventWaitHandle`, represents that behavior. `ManualResetEventSlim` class is a lightweight alternative to `ManualResetEvent`.

On Windows, you can use `EventWaitHandle` for the inter-process synchronization. To do that, create a `EventWaitHandle` instance that represents a named system synchronization event by using one of the `EventWaitHandle` constructors that specifies a name or the `EventWaitHandle.OpenExisting` method.
