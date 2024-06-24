# `throw`, `try`, `catch`, `finally`

## `throw`

`throw;` preserves the original stack trace of the exception, which is stored in the [↑ `Exception.StackTrace`](https://learn.microsoft.com/en-us/dotnet/api/system.exception.stacktrace) property. Opposite to that, `throw exception;` updates the StackTrace property of `exception`.

```csharp
try
{
    throw new Exception("Custom exception");
}
catch (Exception exception)
{
    Console.WriteLine(exception.Message);
    throw;
}
```

When an exception is thrown, the CLR looks for the `catch` block that can handle this exception. If the currently executed method doesn't contain such a `catch` block, the CLR looks at the method that called the current method, and so on up the call stack. If no `catch` block is found, the CLR terminates the executing thread.

For more information, see the [↑ How exceptions are handled](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/exceptions#214-how-exceptions-are-handled) section of the C# language specification.

## `finally`

In almost all cases `finally` blocks are executed. The only cases where `finally` blocks aren't executed involve immediate termination of a program. For example, such a termination might happen because of the [↑ Environment.FailFast](https://learn.microsoft.com/en-us/dotnet/api/system.environment.failfast) call or an [↑ `OverflowException`](https://learn.microsoft.com/en-us/dotnet/api/system.overflowexception) or [↑ `InvalidProgramException`](https://learn.microsoft.com/en-us/dotnet/api/system.invalidprogramexception) exception. Most operating systems perform a reasonable resource clean-up as part of stopping and unloading the process.

The example, when `finally` does not execute:

```csharp
try
{
    M();
}
catch
{
    Console.WriteLine("Won't catch");
}
finally
{
    Console.WriteLine("Won't run");
}

void M()
{
    M();
}

// Output:
// Stack overflow.
// Repeat 523904 times:
// --------------------------------
//    at Program.<<Main>$>g__M|0_0()
// --------------------------------
//    at Program.<Main>$(System.String[])
```

Although this executes both `catch` and `finally` blocks:

```csharp
try
{
    throw new StackOverflowException();
}
catch
{
    Console.WriteLine("Catches");
}
finally
{
    Console.WriteLine("Runs");
}

// Output:
// Catches
// Runs
```

[↑ Exception-handling statements](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/exception-handling-statements).
