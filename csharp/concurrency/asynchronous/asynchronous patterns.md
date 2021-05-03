# Asynchronous patterns

- [Asynchronous patterns](#asynchronous-patterns)
  - [Task-Based Asynchronous Pattern (TAP)](#task-based-asynchronous-pattern-tap)
  - [Asynchronous programming model (APM)](#asynchronous-programming-model-apm)
  - [Event-Based asynchronous programming (EAP)](#event-based-asynchronous-programming-eap)
  - [Continuation Passing Style (CPS)](#continuation-passing-style-cps)
  - [Custom async patterns](#custom-async-patterns)
  - [ISynchronizeInvoke](#isynchronizeinvoke)

## Task-Based Asynchronous Pattern (TAP)

The **Task-Based Asynchronous Pattern** (**TAP**) is the modern asynchronous API pattern that is ready for use with `await`. Each asynchronous operation is represented by a single method that returns an awaitable. An "awaitable" is any type that can be consumed by `await`; this is usually `Task` or `Task<T>` but may also be `ValueTask`, `ValueTask<T>`, a type defined by a framework, or even a custom type defined by a library.

It is common for TAP methods to have an `Async` suffix. However, this is just a convention; not all TAP methods have an `Async` suffix. It can be skipped if the API developer believes the asynchronous context is sufficiently implied; e.g., `Task.WhenAll` and `Task.WhenAny` do not have an `Async` suffix. Furthermore, keep in mind that the `Async` suffix may be present on non-TAP methods (e.g., `WebClient.DownloadStringAsync` is not a TAP method). The usual pattern in this case is for the TAP method to have a `TaskAsync` suffix (e.g., `WebClient.DownloadStringTaskAsync` is a TAP method).

Methods that return asynchronous streams also follow a TAP-like pattern, with `Async` used as a suffix. Even though they don’t return awaitables, they do return awaitable streams — types that can be consumed using `await foreach`.

The Task-Based Asynchronous Pattern can be recognized by these characteristics:

1. The operation is represented by a single method.
2. The method returns an awaitable or an awaitable stream.
3. The method usually ends with `Async`.

Here’s an example of a type with a TAP API:

```csharp
class ExampleHttpClient
{
    public Task<string> GetStringAsync(Uri requestUri);
    
    // Synchronous equivalent, for comparison
    public string GetString(Uri requestUri);
}
```

Consuming the Task-Based Asynchronous Pattern is done using `await`.

## Asynchronous programming model (APM)

## Event-Based asynchronous programming (EAP)

## Continuation Passing Style (CPS)

## Custom async patterns

Very specialized types will sometimes define their own custom asynchronous patterns. Custom patterns do not have any common characteristics and are therefore the hardest to recognize. Thankfully, custom asynchronous patterns are
rare.

Here’s an example of a type with a custom asynchronous API:

```csharp
class MyHttpClient
{
    public void GetString(Uri requestUri, MyHttpClientAsynchronousOperation operation);
    
    // Synchronous equivalent, for comparison
    public string GetString(Uri requestUri);
}
```

`TaskCompletionSource<T>` is the only way to consume custom asynchronous patterns.

## ISynchronizeInvoke

All the previous patterns are for asynchronous operations that are started, and once they start, they complete once. Some components follow a subscription model: they represent a push-based stream of events rather than a single operation that starts once and completes once.

Some components use an `ISynchronizeInvoke` pattern for their events. They expose a single `ISynchronizeInvoke` property, and consumers set that property to an implementation that allows the component to schedule work. This is most commonly used to schedule work to a UI thread so that the component’s events are raised on the UI thread. By convention, if `ISynchronizeInvoke` is `null`, then no synchronizing of the events is done, and they may be raised on background threads.

The `ISynchronizeInvoke` pattern can be recognized by these characteristics:

1. There is a property of type `ISynchronizeInvoke`.
2. The property is usually called `SynchronizingObject`.

Here’s an example of a type that uses the `ISynchronizeInvoke` pattern:

```csharp
class MyHttpClient
{
    public ISynchronizeInvoke SynchronizingObject { get; set; }
    public void StartListening();
    public event Action<string> StringArrived;
}
```

Since `ISynchronizeInvoke` implies multiple events in a subscription model, the proper way to consume these components is to translate those events to an observable stream, either using `FromEvent` or `Observable.Create`.
