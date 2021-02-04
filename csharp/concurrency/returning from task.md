# Returning from Task

```csharp
using System.Collections.Generic;
using System.Threading.Tasks;

class Example
{
    static async Task Main()
    {
        var a = A();
        int b = await B();
    }

    #region with async

    static async Task One()
    {
        await Task.Delay(100);
    }

    static async Task<int> Two()
    {
        await Task.Delay(100);
        return 5;
    }

    static async void Three()
    {
        await Task.Delay(100);
    }

    static async IAsyncEnumerable<int> Four()
    {
        for (int i = 0; i < 10; i++)
        {
            await Task.Delay(100);
            yield return i * 4;
        }
    }

    #endregion

    #region without async

    public static Task A()
    {
        // Creates a Task that will complete after a time delay
        // return Task.Delay(100);

        // Queues the specified work to run on the ThreadPool and returns a Task handle for that work.
        // return Task.Run(() => { });

        // Creates a Task that's completed due to cancellation with the specified token.
        // return Task.FromCanceled(new CancellationToken(true));


        // return Task.FromResult(10);

        return Task.CompletedTask;
    }

    public static Task<int> B()
    {
        return Task.FromResult(10);
    }

    #endregion
}
```
