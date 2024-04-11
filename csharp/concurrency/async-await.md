# `async`/`await`

## Table of contents

- [`async`/`await`](#asyncawait)
  - [Table of contents](#table-of-contents)
  - [Practical advices](#practical-advices)
  - [`async void`](#async-void)
  - [Awaitable types](#awaitable-types)
  - [Create task](#create-task)
  - [Deadlock](#deadlock)

## Practical advices

- Avoid `.Result()` and `.Wait()`. Use `await` for _asynchronous_ code and `.GetAwaiter().GetResult()` for _synchronous_ code. `.GetAwaiter().GetResult()` is effectively the same as `.Wait()`, it's going to block, but it will unwrap the exception, if the exception is thrown inside running method.
- Use `.ConfigureAwait(false)` when the calling thread isn't needed. Most business logic does not need to return to the calling thread.
- Avoid `return await`. When `await` is only used in the `return` statement, return the `Task` instead, but don't do this inside of `try/catch` or `using()` blocks.

[↑ Is awaiting a Task instead of returning it directly in C# actually slower?](https://www.youtube.com/watch?v=Q2zDatDVnO0)

## `async void`

The problem with calling `async void` is that you don't even get the task back. You have no way of knowing when the function's task has completed. You can't `await` an `async void` function too.

```csharp
try
{
    A();
}
catch (Exception e)
{
    Console.WriteLine("This will never be caught");
}

Thread.Sleep(10);
Console.WriteLine("I may get executed too if Thread.Sleep from above won't take too long");

static async void A()
{
    // await Task.Yield();
    throw new Exception("Ooops");
}
```

```output
Unhandled exception. I may get executed too if Thread.Sleep above won't take too long
System.Exception: Ooops
   at <Program>$.<<<Main>$>g__A|0_0>d.MoveNext() in /Users/aleksei/repositories/examples/ConsoleApp1/Program.cs:line 18
--- End of stack trace from previous location ---
   at System.Threading.Tasks.Task.<>c.<ThrowAsync>b__140_1(Object state)
   at System.Threading.QueueUserWorkItemCallbackDefaultContext.Execute()
   at System.Threading.ThreadPoolWorkQueue.Dispatch()
   at System.Threading._ThreadPoolWaitCallback.PerformWaitCallback()

Process finished with exit code 134.
```

The `async void` function is a "fire and forget": you start the task chain, but you don't care about when it's finished.

When the function returns, all you know is that everything up to the first `await` has executed. Everything after the first `await` will run at some unspecified point in the future that you have no access to.

The bottom line is that an `async void` can crash the system and usually should be used only on the UI side event handlers.

The code below will work:

```csharp
M();

Thread.Sleep(2000);

Console.WriteLine("Still works");

async Task M()
{
    await Task.Delay(1000);
    throw new Exception("Boom!");
}
```

But if you replace `Task` with `void` it won't — the process will crash because of the unhandled exception.

## Awaitable types

The `await` keyword is not limited to working with tasks; it can work with any kind of awaitable that follows a certain pattern. As an example, the Base Class Library includes the `ValueTask<T>` type, which reduces memory allocations if the result is commonly synchronous; for example, if the result can be read from an in-memory cache. `ValueTask<T>` is not directly convertible to `Task<T>`, but it does follow the awaitable pattern, so you can directly `await` it. There are other examples, and you can build your own, but most of the time `await` will take a Task or `Task<TResult>`.

[↑ Await anything](https://devblogs.microsoft.com/pfxteam/await-anything/).

## Create task

There are two basic ways to create a `Task` instance. Some tasks represent actual code that a CPU has to execute; these computational tasks should be created by calling `Task.Run` or `TaskFactory.StartNew` if you need them to run on a particular scheduler.

Other tasks represent a notification; these kinds of event-based tasks are created by `TaskCompletionSource<TResult>` or one of its shortcuts. Most I/O tasks use `TaskCompletionSource<TResult>`.

## Deadlock

A **deadlock** is a situation where two or more threads or processes are unable to proceed because each is waiting for the other to release a resource.

There's one other important guideline when it comes to methods: once you start using `async`, it's best to allow it to grow through your code. If you call an `async` method, you should eventually `await` the task it returns. Resist the temptation to call `Task.Wait`, `Task<TResult>.Result`, or `GetAwaiter().GetResult()`; doing so could cause a deadlock. Consider the following method:

```csharp
void Deadlock()
{
    // Start the delay.
    Task task = WaitAsync();
    // Synchronously block, waiting for the async method to complete.
    task.Wait();
}

async Task WaitAsync()
{
    // This await will capture the current context ...
    await Task.Delay(TimeSpan.FromSeconds(1));
    // ... and will attempt to resume the method here in that context.
}
```

The code in this example will deadlock if called from a UI or ASP.NET Classic context because both of those contexts only allow one thread in at a time. `Deadlock` will call `WaitAsync`, which begins the delay. `Deadlock` then synchronously waits for that method to complete, blocking the context thread. When the delay completes, `await` attempts to resume `WaitAsync` within the captured context, but it cannot because there's already a thread blocked in the context, and the context only allows one thread at a time. Deadlock can be prevented two ways: you can use `ConfigureAwait(false)` within `WaitAsync` which causes `await` to ignore its context, or you can `await` the call to `WaitAsync` making `Deadlock` into an `async` method.
