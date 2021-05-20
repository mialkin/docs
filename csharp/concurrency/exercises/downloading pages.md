# Downloading pages

- [Downloading pages](#downloading-pages)
  - [Without throttling](#without-throttling)
  - [With throttling](#with-throttling)
  - [Using `Select` and throttling](#using-select-and-throttling)

## Without throttling

On avarage it takes **8–12** seconds to execute:

```csharp
HttpClient client = new();

Stopwatch stopWatch = new();
stopWatch.Start();

IEnumerable<Task> tasks = CreateTasks();
await Task.WhenAll(tasks);

stopWatch.Stop();

TimeSpan ts = stopWatch.Elapsed;
string elapsedTime = $"{ts.Seconds:00}.{ts.Milliseconds}";

Console.WriteLine(elapsedTime);

async Task DownloadUri(string uri)
{
    HttpResponseMessage response = await client.GetAsync(uri);
    response.EnsureSuccessStatusCode();
    string responseBody = await response.Content.ReadAsStringAsync();
}

IEnumerable<Task> CreateTasks()
{
    List<Task> list = new();
    for (int i = 0; i < 100; i++)
    {
        list.Add(DownloadUri("https://internship.mialkin.ru"));
        list.Add(DownloadUri("https://dict.mialkin.ru"));
    }
    return list;
}
```

## With throttling

On avarage it takes only **2–3** seconds to execute:

```csharp
HttpClient client = new();

Stopwatch stopWatch = new();
stopWatch.Start();

var maxParallel = 200;
var throttler = new SemaphoreSlim(initialCount: maxParallel);

IEnumerable<Task> tasks = CreateTasks();
await Task.WhenAll(tasks);

stopWatch.Stop();

TimeSpan ts = stopWatch.Elapsed;
string elapsedTime = $"{ts.Seconds:00}.{ts.Milliseconds}";

Console.WriteLine(elapsedTime);

async Task DownloadUri(string uri)
{
    await throttler.WaitAsync();
    try
    {
        HttpResponseMessage response = await client.GetAsync(uri);
        response.EnsureSuccessStatusCode();
        string responseBody = await response.Content.ReadAsStringAsync();
    }
    finally
    {
        throttler.Release();
    }
}

IEnumerable<Task> CreateTasks()
{
    List<Task> list = new();
    for (int i = 0; i < 100; i++)
    {
        list.Add(DownloadUri("https://internship.mialkin.ru"));
        list.Add(DownloadUri("https://dict.mialkin.ru"));
    }
    return list;
}
```

## Using `Select` and throttling

On avarage it takes **3–4** seconds to execute:

```csharp
IEnumerable<Task> CreateTasks()
{
    int repeat = 1000;
    IEnumerable<string> urls = Enumerable.Repeat("https://internship.mialkin.ru", repeat)
        .Concat(Enumerable.Repeat("https://dict.mialkin.ru", repeat));
    
    IEnumerable<Task> enumerable = urls.Select(async x => {
        await DownloadUri(x);
    });

    return enumerable;
}
```
