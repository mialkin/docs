# Event

Events enable a class or object to notify other classes or objects when something of interest occurs. The class that sends (or raises) the event is called the _publisher_ and the classes that receive (or handle) the event are called _subscribers_.

All events in the .NET class library are based on the `EventHandler` [delegate](delegate.md), which is defined as follows:

```csharp
public delegate void EventHandler(object sender, EventArgs e);
```

Although events in classes that you define can be based on any valid [delegate](delegate.md) type, even delegates that return a value, it is generally recommended that you base your events on the .NET pattern by using `EventHandler`.

The name `EventHandler` can lead to a bit of confusion as it doesn't actually handle the event. The `EventHandler`, and generic `EventHandler<TEventArgs>` are delegate types. A method or lambda expression whose signature matches the delegate definition is the event handler and will be invoked when the event is raised.

## EventArgs Class

Represents the base class for classes that contain event data, and provides a value to use for events that do not include event data.

```csharp
using System;

namespace System
{
    // The base class for all event classes.
    [Serializable]
    [System.Runtime.CompilerServices.TypeForwardedFrom("mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")]
    public class EventArgs
    {
        public static readonly EventArgs Empty = new EventArgs();

        public EventArgs()
        {
        }
    }
}
```

To create a custom event data class, create a class that derives from the `EventArgs` class and provide the properties to store the necessary data. The name of your custom event data class should end with `EventArgs`.

To pass an object that does not contain any data, use the `Empty` field.

## Links

[↑ event (C# reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/event)

[↑ EventArgs Class](https://docs.microsoft.com/en-us/dotnet/api/system.eventargs)

[↑ How to: Raise and Consume Events](https://docs.microsoft.com/en-us/dotnet/standard/events/how-to-raise-and-consume-events)
