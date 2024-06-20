# `Channel`

The [↑ `Channel`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.channels.channel) class provides static methods for creating channels.

```csharp
var channel = Channel.CreateBounded<int>(10);

_ = Task.Run(async () =>
{
    var counter = 1;
    while (true)
    {
        await channel.Writer.WriteAsync(counter++);
        await Task.Delay(2000);
    }
});

_ = Task.Run(async () =>
{
    var counter = -1;
    while (true)
    {
        await channel.Writer.WriteAsync(counter--);
        await Task.Delay(1000);
    }
});


var task = Task.Run(async () =>
{
    while (true)
    {
        var reader = channel.Reader;
        Console.WriteLine($"Available items in the channel: {reader.Count}");
        Console.WriteLine($"Read value: {await reader.ReadAsync()}");
        await Task.Delay(3000);
    }
});

// Output:
// Available items in the channel: 2
// Read value: 1
// Available items in the channel: 5
// Read value: -1
// Available items in the channel: 9
// Read value: -2
// Available items in the channel: 10
// Read value: -3
// Available items in the channel: 10
// Read value: 2
// Available items in the channel: 10
// Read value: -4
// Available items in the channel: 10
// Read value: -5
```

## `Open.ChannelExtensions` library

The [↑ `Open.ChannelExtensions`](https://github.com/Open-NET-Libraries/Open.ChannelExtensions) library is a set of extension methods for optimizing/simplifying `System.Threading.Channels` usage.

[↑ An Introduction to `System.Threading.Channels`](https://devblogs.microsoft.com/dotnet/an-introduction-to-system-threading-channels).

[↑ Producer/consumer pipelines with System.Threading.Channels](https://blog.maartenballiauw.be/post/2020/08/26/producer-consumer-pipelines-with-system-threading-channels.html).