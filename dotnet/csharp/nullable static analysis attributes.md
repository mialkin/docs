# Nullable static analysis attributes

You can apply a number of attributes that provide information to the compiler about the semantics of your APIs. That information helps the compiler perform static analysis and determine when a variable is not `null`.

All the examples assume C# 8.0 or newer, and the code is in a [↑ nullable context](https://docs.microsoft.com/en-us/dotnet/csharp/nullable-references#nullable-contexts).

Imagine your library has the following API to retrieve a resource string:

```csharp
bool TryGetMessage(string key, out string message)
```

This API has the following rules relating to the nullness of these arguments:

- Callers shouldn't pass `null` as the argument for `key`.
- Callers can pass a variable whose value is `null` as the argument for `message`.
- If the `TryGetMessage` method returns `true`, the value of `message` isn't `null`. If the return value is `false`, the value of `message` is `null`.

## Links

[↑ System.Diagnostics.CodeAnalysis Namespace](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.codeanalysis)

[↑ Nullable static analysis](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/attributes/nullable-analysis)
