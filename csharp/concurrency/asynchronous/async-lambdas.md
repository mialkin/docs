# Async lambdas

```csharp
HttpClient client = new();

await Task.Run(async () =>
{
    HttpResponseMessage response = await client.GetAsync("https://example.com");

    string text = await response.Content.ReadAsStringAsync();
    Console.WriteLine(text);
});
```

## Catching exceptions

```csharp
try
{
    await Task.Run(async () => await DoWork()); // Task.Run accepts Func<Task?> here.
}
catch (Exception ex)
{
    Console.WriteLine("Caught exception: " + ex.Message);
}

async Task DoWork()
{
    await Task.Delay(1000);
    throw new Exception("Exception inside DoWork method");
}
```

Output:

```console
Caught exception: Exception inside DoWork method
````
