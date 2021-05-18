# Async lambdas

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static readonly HttpClient Client = new HttpClient();

    static async Task Main()
    {
        string url = "https://example.com";

        await Task.Run(async () =>
        {
            HttpResponseMessage response = await Client.GetAsync(url);

            string text = await response.Content.ReadAsStringAsync();
            Console.WriteLine(text);
        });

    }
}
```
