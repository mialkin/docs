# Polly

[↑ Polly](https://github.com/App-vNext/Polly) is a .NET resilience and transient-fault-handling library that allows developers in a fluent and thread-safe manner to express such policies as:

Reactive policies:

- Retry
- Circuit breaker
- Fallback

Proactive policies:

- Timeout
- Bulkhead
- Rate limiting

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

First the exceptions that need to be handled by the policy need to be specified:

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

### Rate limiting
