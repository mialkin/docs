# `HttpClient`

```csharp
using System.Net.Http;

HttpClient client = new();

HttpResponseMessage response = await client.GetAsync("https://mialkin.ru");
response.EnsureSuccessStatusCode();

string responseBody = await response.Content.ReadAsStringAsync();
```

[↑ Microsoft documentation](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclient)
