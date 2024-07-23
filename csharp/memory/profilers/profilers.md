# dotMemory

[↑ dotMemory](https://www.jetbrains.com/dotmemory) is a .NET memory profiler.

## Generating large strings to fill up LOH

Memory diagram after calling `/api/generate-large-strings` endpoint 3 times:

<img src="images/dotmemory-large-strings.jpg" alt="Memory diagram" />

Set memory limit to 300 MB using [↑ `System.GC.HeapHardLimit`](https://learn.microsoft.com/en-us/dotnet/core/runtime-config/garbage-collector#heap-limit) setting inside [↑ `runtimeconfig.template.json`](https://learn.microsoft.com/en-us/dotnet/core/runtime-config/#runtimeconfigjson) config:

```json
{
  "configProperties": {
    "System.GC.HeapHardLimit": 300000000
  }
}
```

Code of `/api/generate-large-strings` endpoint:

```csharp
public static class GenerateLargeStringsEndpoint
{
    public static void GenerateLargeStrings(this IEndpointRouteBuilder builder, string routePattern)
    {
        builder.MapPost(routePattern, (ILoggerFactory loggerFactory) =>
            {
                var logger = loggerFactory.CreateLogger(nameof(GenerateLargeStringsEndpoint));
                logger.LogInformation(
                    $"TotalAvailableMemoryBytes: {GC.GetGCMemoryInfo().TotalAvailableMemoryBytes}");

                var list = new List<string>();
                var counter = 1;

                while (true)
                {
                    var randomString = RandomNumberGenerator.GetString(
                        choices: "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789",
                        length: 10_000_000);

                    list.Add(randomString);
                    logger.LogInformation("Added string " + counter);

                    counter++;

                    Thread.Sleep(500);

                    if (counter > 10)
                    {
                        break;
                    }
                }

                logger.LogInformation("Broke the loop");

                return Results.Ok();
            })
            .WithOpenApi(x => new OpenApiOperation(x) { Summary = "Generate large strings" });
    }
}

// Output:
// 
// [22:22:05 INF] TotalAvailableMemoryBytes: 300000000
// [22:22:07 INF] Added string 1
// [22:22:09 INF] Added string 2
// [22:22:12 INF] Added string 3
// [22:22:14 INF] Added string 4
// [22:22:16 INF] Added string 5
// [22:22:19 INF] Added string 6
// [22:22:21 INF] Added string 7
// [22:22:23 INF] Added string 8
// [22:22:25 INF] Added string 9
// [22:22:27 INF] Added string 10
// [22:22:28 INF] Broke the loop
```
