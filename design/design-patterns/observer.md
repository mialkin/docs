# Observer

The **Observer** is is a behavioral software design pattern that defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

## Participants

- **`Subject`**
  - knows its observers. Any number of `Observer` objects may observe a subject
  - provides an interface for attaching and detaching `Observer` objects
- **`Observer`**
  - defines an updating interface for objects that should be notified of changes in a subject
- **`ConcreteSubject`**
  - stores state of interest to `ConcreteObserver` objects
  - sends a notification to its observers when its state changes
- **`ConcreteObserver`**
  - maintains a reference to a `ConcreteSubject` object
  - stores state that should stay consistent with the subject's
  - implements the Observer updating interface to keep its state consistent with the subject's

## C# implementation

```csharp
using System;
using System.Collections.Generic;

IObserver observer1 = new Observer("1");
IObserver observer2 = new Observer("2");

ISubject subject = new Subject();
subject.Attach(observer1);
subject.Notify();

subject.Attach(observer2);
subject.Notify();

subject.Detach(observer1);
subject.Notify();

interface IObserver
{
    void Update();
}

class Observer : IObserver
{
    public string Name { get; }
    public Observer(string name) => Name = name;
    public void Update() => Console.WriteLine($"Subject notified observer {Name}");
}

interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify();
}

class Subject : ISubject
{
    private readonly List<IObserver> _observers = new();

    public void Attach(IObserver observer)
    {
        if (_observers.Contains(observer))
            return;

        _observers.Add(observer);
    }

    public void Detach(IObserver observer)
    {
        if (_observers.Contains(observer))
            _observers.Remove(observer);
    }

    public void Notify()
    {
        foreach (IObserver observer in _observers)
            observer.Update();

        Console.WriteLine("--------");
    }
}
```

Output:

```output
Subject notified observer 1
--------
Subject notified observer 1
Subject notified observer 2
--------
Subject notified observer 2
--------
```

## .NET interfaces

[↑ `IObservable<T>` interface](https://docs.microsoft.com/en-us/dotnet/api/system.iobservable-1)

```csharp
namespace System
{
    public interface IObservable<out T>
    {
        IDisposable Subscribe(IObserver<T> observer);
    }
}
```

[↑ `IObserver<T>` interface](https://docs.microsoft.com/en-us/dotnet/api/system.iobserver-1)

```csharp
namespace System
{
    public interface IObserver<in T>
    {
        void OnNext(T value);
        void OnError(Exception error);
        void OnCompleted();
    }
}
```
