# Downloading pages

Without throttling it takes **8–12** seconds to execute on avarage:

```csharp
HttpClient client = new();

Stopwatch stopWatch = new();
stopWatch.Start();

List<Task> tasks = CreateTasks();
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

List<Task> CreateTasks()
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

With throttling it takes only **2–3** seconds to execute on avarage:

```csharp
HttpClient client = new();

Stopwatch stopWatch = new();
stopWatch.Start();

var maxParallel = 200;
var throttler = new SemaphoreSlim(initialCount: maxParallel);

List<Task> tasks = CreateTasks();
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

List<Task> CreateTasks()
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
