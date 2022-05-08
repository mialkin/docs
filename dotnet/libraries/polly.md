# Polly

[↑ Polly](https://github.com/App-vNext/Polly) is a .NET resilience and transient-fault-handling library that helps navigate the unreliable network by providing *resilience strategies* in fluent-to-express *policies*:

Reactive policies:

- Retry
- Circuit breaker
- Fallback

Proactive policies:

- Timeout
- Bulkhead
- Rate limiting

Example usages are fault-tolerance for any distributed systems and inter-process calls, such as WCF, RESTful calls between microservices, calls to cloud services, Internet of Things connectivity, messaging systems, calls to your persistence layer, for example,wrapping Entity Framework calls, etc.

## Table of contents

- [Polly](#polly)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Reactive policies](#reactive-policies)
    - [Retry](#retry)
    - [Circuit breaker](#circuit-breaker)
    - [Fallback](#fallback)
  - [Proactive policies](#proactive-policies)
    - [Timeout](#timeout)
    - [Bulkhead](#bulkhead)
    - [Rate limiting](#rate-limiting)

## Installation

```bash
dotnet add package Polly
```

## Reactive policies

Reactive, aka fault-handling, policies handle specific exceptions thrown by, or results returned by the delegates you execute through the policy.

1\. First specify the exceptions, that need to be handled by the policy:

```csharp
// Single exception type
Policy
  .Handle<HttpRequestException>()

// Single exception type with condition
Policy
  .Handle<SqlException>(ex => ex.Number == 1205)

// Multiple exception types
Policy
  .Handle<HttpRequestException>()
  .Or<OperationCanceledException>()

// Multiple exception types with condition
Policy
  .Handle<SqlException>(ex => ex.Number == 1205)
  .Or<ArgumentException>(ex => ex.ParamName == "example")

// Inner exceptions of ordinary exceptions or AggregateException, with or without conditions
// (HandleInner matches exceptions at both the top-level and inner exceptions)
Policy
  .HandleInner<HttpRequestException>()
  .OrInner<OperationCanceledException>(ex => ex.CancellationToken != myToken)
```

Then, optionally, specify the return results that needs to be handled:

```csharp
// Handle return value with condition
Policy
  .HandleResult<HttpResponseMessage>(r => r.StatusCode == HttpStatusCode.NotFound)

// Handle multiple return values
Policy
  .HandleResult<HttpResponseMessage>(r => r.StatusCode == HttpStatusCode.InternalServerError)
  .OrResult(r => r.StatusCode == HttpStatusCode.BadGateway)

// Handle primitive return values (implied use of .Equals())
Policy
  .HandleResult<HttpStatusCode>(HttpStatusCode.InternalServerError)
  .OrResult(HttpStatusCode.BadGateway)

// Handle both exceptions and return values in one policy
HttpStatusCode[] httpStatusCodesWorthRetrying = {
   HttpStatusCode.RequestTimeout, // 408
   HttpStatusCode.InternalServerError, // 500
   HttpStatusCode.BadGateway, // 502
   HttpStatusCode.ServiceUnavailable, // 503
   HttpStatusCode.GatewayTimeout // 504
};

HttpResponseMessage result = await Policy
  .Handle<HttpRequestException>()
  .OrResult<HttpResponseMessage>(r => httpStatusCodesWorthRetrying.Contains(r.StatusCode))
  .RetryAsync(...)
  .ExecuteAsync( /* some Func<Task<HttpResponseMessage>> */ )
```

2\. Then, specify how the policy should be handle faults with `Retry`, `RetryForever`, `WaitAndRetry`, `WaitAndRetryForever`, `CircuitBreaker`, `AdvancedCircuitBreaker`, `Fallback`.

3\. Execute an `Action`, `Func`, or lambda delegate equivalent, through the policy using `ExecuteAsync` method.

### Retry

```csharp
using Polly;
using Polly.Retry;

const int maxRetries = 3;
AsyncRetryPolicy<ApiResult> retryPolicy = Policy<ApiResult>
    .Handle<HttpRequestException>(exception => exception.Message != "Forest") // Do not handle exception and just throw if this condition is not met.
    .RetryAsync(3);
    // .RetryForeverAsync();
    // .WaitAndRetryAsync(maxRetries, times => TimeSpan.FromSeconds(times * 2)); // Exponential retry time.

var service = new ApiService();
var result = await retryPolicy.ExecuteAsync(service.Call);

Console.WriteLine(result.Message);

public class ApiService
{
    private int _counter;

    public async Task<ApiResult> Call()
    {
        // Call actual API.
        await Task.Delay(TimeSpan.FromSeconds(1));

        Console.WriteLine($"API called times: {++_counter}");

        // throw new Exception(); // This will crash the app after the 1-st API call.
        // throw new HttpRequestException(); // This will crash the app after the 4-th API call.
        // throw new HttpRequestException("Forest"); // This will crash the app after the 1-st API call.

        return new ApiResult { Message = "Sea" };
    }
}

public class ApiResult
{
    public string? Message { get; set; }
}
```

### Circuit breaker

### Fallback

```csharp
// Provide a substitute value, if an execution faults.
Policy<UserAvatar>
   .Handle<FooException>()
   .OrResult(null)
   .Fallback<UserAvatar>(UserAvatar.Blank)

// Specify a func to provide a substitute value, if execution faults.
Policy<UserAvatar>
   .Handle<FooException>()
   .OrResult(null)
   .Fallback<UserAvatar>(() => UserAvatar.GetRandomAvatar()) // where: public UserAvatar GetRandomAvatar() { ... }

// Specify a substitute value or func, calling an action (eg for logging) if the fallback is invoked.
Policy<UserAvatar>
   .Handle<FooException>()
   .Fallback<UserAvatar>(UserAvatar.Blank, onFallback: (exception, context) =>
    {
        // Add extra logic to be called when the fallback is invoked, such as logging
    });
```

## Proactive policies

The proactive policies add resilience strategies that are not based on handling faults which the governed code may throw or return.

### Timeout

Throws `Polly.Timeout.TimeoutRejectedException`:

```csharp
using Polly;
using Polly.Timeout;

var policy = Policy.TimeoutAsync(TimeSpan.FromSeconds(3), TimeoutStrategy.Pessimistic);

await policy.ExecuteAsync(async () =>
{
    Console.WriteLine("Start executing");
    await Task.Delay(TimeSpan.FromSeconds(4));
    Console.WriteLine("Finish executing");
});
```

**Optimistic timeout** operates via `CancellationToken` and assumes delegates you execute support co-operative cancellation. You must use `Execute/Async(...)` overloads taking a `CancellationToken`, and the executed delegate must honor that `CancellationToken`.

**Pessimistic timeout** allows calling code to "walk away" from waiting for an executed delegate to complete, even if it does not support cancellation. In synchronous executions this is at the expense of an extra thread.

### Bulkhead

> A bulkhead is a wall within a ship which separates one compartment from another, such that damage to one compartment does not cause the whole ship to sink.

Similarly, a bulkhead isolation policy constrains operations, such that one faulting channel of actions cannot swamp all resource (threads/CPU/whatever) in a system, bringing down other operations with it. The impact of a faulting system is isolated to the resource-pool to which it is limited; other threads/pools/capacity remain to continue serving other calls.

```csharp
// Restrict executions through the policy to a maximum of twelve concurrent actions.
Policy
  .Bulkhead(12)

// Restrict executions through the policy to a maximum of twelve concurrent actions,
// with up to two actions waiting for an execution slot in the bulkhead if all slots are taken.
Policy
  .Bulkhead(12, 2)

// Restrict concurrent executions, calling an action if an execution is rejected
Policy
  .Bulkhead(12, context =>
    {
        // Add callback logic for when the bulkhead rejects execution, such as logging
    });

// Monitor the bulkhead available capacity, for example for health/load reporting.
var bulkhead = Policy.Bulkhead(12, 2);
// ...
int freeExecutionSlots = bulkhead.BulkheadAvailableCount;
int freeQueueSlots     = bulkhead.QueueAvailableCount;
```

Bulkhead policies throw `BulkheadRejectedException` if items are queued to the bulkhead when the bulkhead execution and queue are both full.

### Rate limiting
