# `event`

The `event` keyword is used to declare an _event_ in a _publisher class_.

## Table of contents

- [`event`](#event)
  - [Table of contents](#table-of-contents)
  - [Event](#event-1)
  - [`event` vs `delegate`](#event-vs-delegate)
  - [Event names guidelines](#event-names-guidelines)
  - [Example](#example)
  - [`EventHandler` delegate](#eventhandler-delegate)
  - [`EventHandler<TEventArgs>` delegate](#eventhandlerteventargs-delegate)

## Event

An **event** is a special kind of [multicast delegate](/csharp/types/delegate.md#multicastdelegate) that can only be invoked from within the class, or derived classes, or `struct` where they are declared, the publisher class. If other classes or structures subscribe to the event, their event handler methods will be called when the publisher class raises the event.

Events enable a class or object to notify other classes or objects when something of interest occurs.

The class that raises the event is called **publisher** and the classes that handle the event are called **subscribers**.

## `event` vs `delegate`

You can subscribe to `event` and to `delegate`. You can raise `event` and call `delegate`. So why bother with events? You can't call `event` from outside of the class, but you can call `delegate`, if it's public.

You can also overwrite `delegate` with `=` operator, while with `event` you can use only `+=` and `-=`. To remove all subscribers from event you need to set event variable to `null`. A `null` reference is the canonical way of representing an empty invocation list, effectively.

You can write a public method on the class you want the event to fire from and fire the event when it is called. You can then call this method from whatever user of your class. Of course, this ruins encapsulation and is bad design.

Field-like events and public fields of delegate types look similar, but are actually very different.

An event is fundamentally like a property — it's a pair of [↑ `add`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/add)/[↑ `remove`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/remove) methods (instead of the `get`/`set` of a property). When you declare a field-like event (i.e. one where you don't specify the `add`/`remove` bits yourself) a public event is created, and a private backing field. This lets you raise the event privately, but allow public subscription. With a public delegate field, _anyone_ can remove other people's event handlers, raise the event themselves, etc — it's an encapsulation disaster.

[↑ Why do we need the "event" keyword while defining events?](https://stackoverflow.com/questions/3028724/why-do-we-need-the-event-keyword-while-defining-events).

## Event names guidelines

Events always refer to some action, either one that is happening or one that has occurred. Therefore, as with methods, events are named with verbs, and verb tense is used to indicate the time when the event is raised.

✔️ DO name events with a verb or a verb phrase.

Examples include `Clicked`, `Painting`, `DroppedDown`, and so on.

✔️ DO give events names with a concept of before and after, using the present and past tenses.

For example, a close event that is raised before a window is closed would be called `Closing`, and one that is raised after the window is closed would be called `Closed`.

❌ DO NOT use "Before" or "After" prefixes or postfixes to indicate pre- and post-events. Use present and past tenses as just described.

✔️ DO name event handlers (delegates used as types of events) with the "EventHandler" suffix, as shown in the following example:

```csharp
public delegate void ClickedEventHandler(object sender, ClickedEventArgs e);
```

✔️ DO use two parameters named `sender` and `e` in event handlers.

The `sender` parameter represents the object that raised the event. The `sender` parameter is typically of type `object`, even if it is possible to employ a more specific type.

✔️ DO name event argument classes with the "EventArgs" suffix.

[↑ Names of Events](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-type-members#names-of-events).

## Example

`PrintingEventHandler.cs`:

```csharp
public delegate void PrintingEventHandler(object sender, string text);
```

`PrintedEventHandler.cs`:

```csharp
public delegate void PrintedEventHandler(object sender, TimeSpan duration);
```

`Printer.cs`:

```csharp
public class Printer
{
    public event PrintingEventHandler? Printing;
    public event PrintedEventHandler? Printed;

    protected virtual void OnPrinting(string message)
    {
        Printing?.Invoke(this, message);
    }

    protected virtual void OnPrinted(TimeSpan duration)
    {
        Printed?.Invoke(this, duration);
    }

    public void Print(string text)
    {
        OnPrinting(text);

        Console.WriteLine(text);

        var duration = TimeSpan.FromSeconds(Random.Shared.Next(1, 60));
        OnPrinted(duration);
    }
}
```

`Program.cs`:

```csharp
var printer = new Printer();

printer.Printing += (sender, text) =>
{
    Console.WriteLine("Received printing event");
    Console.WriteLine($"Printing text: \'{text}\'");
};

printer.Printed += (sender, duration) =>
{
    Console.WriteLine("Received printed event");
    Console.WriteLine($"Duration: \'{duration}\'");
};

printer.Print("War and Peace");
```

Output:

```console
Received printing event
Printing text: 'War and Peace'
War and Peace
Received printed event
Duration: '00:00:42'
```

## `EventHandler` delegate

The `EventHandler` delegate is a predefined delegate that specifically represents an event handler method for an event that does not generate data:

```csharp
public delegate void EventHandler(object? sender, EventArgs e);
```

If the event does not generate event data, the second parameter is simply the value of the [↑ `EventArgs.Empty`](https://learn.microsoft.com/en-us/dotnet/api/system.eventargs.empty) field. Otherwise, the second parameter is a type derived from `EventArgs` and supplies any fields or properties needed to hold the event data.

## `EventHandler<TEventArgs>` delegate

The `EventHandler<TEventArgs>` delegate represents the method that will handle an event when the event provides data:

```csharp
public delegate void EventHandler<TEventArgs>(object? sender, TEventArgs e);
```

If your event does generate data, you must use the generic `EventHandler<TEventArgs>` delegate class:

`PrintingEventArgs.cs`:

```csharp
public class PrintingEventArgs(string message) : EventArgs
{
    public string Message { get; private set; } = message;
}
```

`PrintedEventArgs.cs`:

```csharp
public class PrintedEventArgs(TimeSpan duration) : EventArgs
{
    public TimeSpan Duration { get; private set; } = duration;
}
```

`Printer.cs`:

```csharp
public class Printer
{
    public event EventHandler<PrintingEventArgs>? Printing;
    public event EventHandler<PrintedEventArgs>? Printed;

    protected virtual void OnPrinting(string message)
    {
        Printing?.Invoke(this, new PrintingEventArgs(message));
    }

    protected virtual void OnPrinted(TimeSpan duration)
    {
        Printed?.Invoke(this, new PrintedEventArgs(duration));
    }

    public void Print(string text)
    {
        OnPrinting(text);

        Console.WriteLine(text);

        var duration = TimeSpan.FromSeconds(Random.Shared.Next(1, 60));
        OnPrinted(duration);
    }
}
```

`Program.cs`:

```csharp
var printer = new Printer();

printer.Printing += (sender, printingEventArgs) =>
{
    Console.WriteLine("Received printing event");
    Console.WriteLine($"Printing text: \'{printingEventArgs.Message}\'");
};

printer.Printed += (sender, printedEventArgs) =>
{
    Console.WriteLine("Received printed event");
    Console.WriteLine($"Duration: \'{printedEventArgs.Duration}\'");
};

printer.Print("War and Peace");
```

Output:

```console
Received printing event
Printing text: 'War and Peace'
War and Peace
Received printed event
Duration: '00:00:50'
```
