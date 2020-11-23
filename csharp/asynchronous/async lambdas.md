# Async lamdas

This program will exit before the response from example.com is received:

```csharp
using System;
using System.Net.Http;

class Program
{
    static readonly HttpClient Client = new HttpClient();

    static void Main()
    {
        M(async x =>
        {
            HttpResponseMessage response = await Client.GetAsync(x);

            string text = await response.Content.ReadAsStringAsync();
            Console.WriteLine(text);
        });

        //Thread.Sleep(3000);
        Console.WriteLine("The end");
    }

    static void M(Action<string> action)
    {
        action("http://example.com");
    }
}
```

Output:

```text
The end
```

To make above code work you can uncomment line with `Tread.Sleep`.

The the right way of doing things would be:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static readonly HttpClient Client = new HttpClient();

    static async Task Main()
    {
        string x = "https://example.com";

        await Task.Run(async () =>
        {
            HttpResponseMessage response = await Client.GetAsync(x);

            string text = await response.Content.ReadAsStringAsync();
            Console.WriteLine(text);
        });

        Console.WriteLine("The end");
    }
}
```

Output:

```csharp
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is for use in illustrative examples in documents. You may use this
    domain in literature without prior coordination or asking for permission.</p>
    <p><a href="https://www.iana.org/domains/example">More information...</a></p>
</div>
</body>
</html>

The end

```

## Links

* [↑ Async lambdas](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions#async-lambdas)
* [↑ Potential pitfalls to avoid when passing around async lambdas](https://devblogs.microsoft.com/pfxteam/potential-pitfalls-to-avoid-when-passing-around-async-lambdas/)
