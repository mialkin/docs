# Polly

[↑ Polly](https://github.com/App-vNext/Polly) is a .NET resilience and transient-fault-handling library that allows developers in a fluent and thread-safe manner to express such policies as:

- Retry
- Circuit breaker
- Timeout
- Bulkhead
- Rate limiting
- Fallback

## Table of contents

- [Polly](#polly)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Retry](#retry)
  - [Timeout](#timeout)

## Installation

```bash
dotnet add package Polly
```

## Retry

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

## Timeout

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
