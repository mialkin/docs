# Exceptions in Tasks

## Example 1

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

Output:

```output
Exception of type 'System.Exception' was thrown.
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
            Task.Run(() => throw new Exception()).GetAwaiter().GetResult();
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }
}
```

## Example 2

