# Exceptions handling with `async` and `await`

Error handling is natural with `async` and `await`. In the code snippet that follows, `PossibleExceptionAsync` may throw a `NotSupportedException`, but `TrySomethingAsync` can catch the exception naturally. The caught exception has its stack trace properly preserved and isn't artificially wrapped in a `TargetInvocationException` or `AggregateException`:

```csharp
async Task TrySomethingAsync()
{
    try
    {
        await PossibleExceptionAsync();
    }
    catch (NotSupportedException ex)
    {
        LogException(ex);
        throw;
    }
}
```

When an `async` method throws (or propagates) an exception, the exception is placed on its returned `Task` and the `Task` is completed. When that `Task` is awaited, the await operator will retrieve that exception and (re)throw it in a way such that its original stack trace is preserved. Thus, code such as the following example would work as expected if `PossibleExceptionAsync` was an `async` method:

```csharp
async Task TrySomethingAsync()
{
    // The exception will end up on the Task, not thrown directly.
    Task task = PossibleExceptionAsync();

    try
    {
        // The Task's exception will be raised here, at the await.
        await task;
    }
    catch (NotSupportedException ex)
    {
        LogException(ex);
        throw;
    }
}
```

The following program exits succesfully without catching exception:

```csharp
using System;
using System.Threading.Tasks;

class Example
{
    static void Main()
    {
        try
        {
            Task.Run(() => throw new Exception());
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }
}
```

Catches exception:

```csharp
using System;
using System.Threading.Tasks;

class Example
{
    static async Task Main()
    {
        try
        {
            await Task.Run(() => throw new Exception());
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }
}
```

Also catches exception:

```csharp
using System;
using System.Threading.Tasks;

class Example
{
    static void Main()
    {
        try
        {
            Task.Run(() => throw new Exception())
                .GetAwaiter()
                .GetResult();
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }
}
```
