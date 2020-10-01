# Memento

The **Memento** is is a behavioral software design pattern that, without violating encapsulation, captures and externalizes an object'sinternal state so that the object can be restored to this state later.

## Participants

* **Memento**
  * stores internal state of the Originator object. The memento may store as much or as little of the originator's internal state as necessary at its originator's discretion.
  * protects against access by objects other than the originator. Mementos have effectively two interfaces. Caretaker sees a narrow interface to the Memento — it can only pass the memento to other objects. Originator, in contrast, sees a wide interface, one that lets it access all the data necessary to restore itself to its previous state. Ideally, only the originator that produced the memento would be permitted to access the memento's internal state.
* **Originator**
  * creates a memento containing a snapshot of its current internal state.
  * uses the memento to restore its internal state.
* **Caretaker** (undo mechanism)
  * is responsible for the memento's safekeeping.
  * never operates on or examines the contents of a memento.

## C# implementation

```csharp
using System;

class Caretaker
{
    public static void Main()
    {
        IOriginator originator = new Originator(1, 5, "One");

        Memento memento = originator.CreateMemento();

        Console.WriteLine(
            $"Originator before state change: X={originator.X}, Y={originator.Y}, Name={originator.Name}");

        originator.UpdateState(0, 0);

        Console.WriteLine(
            $"Originator after state change: X={originator.X}, Y={originator.Y}, Name={originator.Name}");

        originator.RestoreStateFromMemento(memento);

        Console.WriteLine(
            $"Originator after state restore: X={originator.X}, Y={originator.Y}, Name={originator.Name}");
    }
}

interface IOriginator
{
    public int X { get; }
    public int Y { get; }
    public string Name { get; }
    public void UpdateState(int x, int y);
    Memento CreateMemento();
    void RestoreStateFromMemento(Memento memento);
}

class Originator : IOriginator
{
    public int X { get; private set; }
    public int Y { get; private set; }

    public string Name { get; }

    public Originator(int x, int y, string name)
    {
        X = x;
        Y = y;
        Name = name;
    }

    public void UpdateState(int x, int y)
    {
        X = x;
        Y = y;
    }

    public Memento CreateMemento()
    {
        return new Memento(X, Y);
    }

    public void RestoreStateFromMemento(Memento memento)
    {
        X = memento.X;
        Y = memento.Y;
    }
}

class Memento
{
    public int X { get; }
    public int Y { get; }

    public Memento(int x, int y)
    {
        X = x;
        Y = y;
    }
}
```

Output:

```output
Originator before state change: X=1, Y=5, Name=One
Originator after state change: X=0, Y=0, Name=One
Originator after state restore: X=1, Y=5, Name=One
```
