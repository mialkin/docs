# Asynchronous patterns

## Task-based asynchronous pattern (TAP)

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
