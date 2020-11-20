# Properties of `Task` class

## Task.CurrentId

Returns the ID of the currently executing `Task` — an integer that was assigned by the system to the currently-executing task..

```csharp
public static int? CurrentId { get; }
```

`CurrentId` is a static property that is used to get the identifier of the currently executing task from the code that the task is executing. It differs from the `Id` property, which returns the identifier of a particular `Task` instance. If you attempt to retrieve the `CurrentId` value from outside the code that a task is executing, the property returns `null`.

Note that although collisions are very rare, task identifiers are not guaranteed to be unique.
