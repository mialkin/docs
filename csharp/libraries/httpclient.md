# `HttpClient`

`HttpClient` is intended to be instantiated once and re-used throughout the life of an application. Instantiating an `HttpClient` class for every request will exhaust the number of sockets available under heavy loads.

```csharp
using System.Net.Http;

HttpClient client = new();

HttpResponseMessage response = await client.GetAsync("https://mialkin.ru");
response.EnsureSuccessStatusCode();

string responseBody = await response.Content.ReadAsStringAsync();
```

## Incorrect usage

Experienced .NET developers know to call `Dispose` on objects that implement `IDisposable`. Not disposing objects that implement `IDisposable` typically results in leaked memory or leaked system resources.

`HttpClient` implements `IDisposable`, but should not be disposed on every invocation. Rather, `HttpClient` should be reused.

The following endpoint creates and disposes a new `HttpClient` instance on every request:

```csharp
[HttpGet("httpclient1")]
public async Task<int> GetHttpClient1(string url)
{
    using (var httpClient = new HttpClient())
    {
        var result = await httpClient.GetAsync(url);
        return (int)result.StatusCode;
    }
}
```

Under load, the following error messages are logged:

```terminal
fail: Microsoft.AspNetCore.Server.Kestrel[13]
      Connection id "0HLG70PBE1CR1", Request id "0HLG70PBE1CR1:00000031":
      An unhandled exception was thrown by the application.
System.Net.Http.HttpRequestException: Only one usage of each socket address
    (protocol/network address/port) is normally permitted --->
    System.Net.Sockets.SocketException: Only one usage of each socket address
    (protocol/network address/port) is normally permitted
   at System.Net.Http.ConnectHelper.ConnectAsync(String host, Int32 port,
    CancellationToken cancellationToken)
```

Even though the `HttpClient` instances are disposed, the actual network connection takes some time to be released by the operating system. By continuously creating new connections, ports exhaustion occurs. Each client connection requires its own client port.

One way to prevent port exhaustion is to reuse the same `HttpClient` instance:

```csharp
private static readonly HttpClient _httpClient = new HttpClient();

[HttpGet("httpclient2")]
public async Task<int> GetHttpClient2(string url)
{
    var result = await _httpClient.GetAsync(url);
    return (int)result.StatusCode;
}
```

The `HttpClient` instance is released when the app stops. This example shows that not every disposable resource should be disposed after each use.

[↑ Microsoft documentation](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclient)
