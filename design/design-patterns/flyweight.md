# Flyweight

The **Flyweight** is a structural software design pattern that uses sharing to support large numbers of fine-grained objects efficiently.

> **Flyweight** is a structural design pattern that lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each object.
> –– <cite>[Refactoring.guru website][1]</cite>

[1]: https://refactoring.guru/design-patterns/flyweight

## Participants

* **Flyweight**
  * declares an interface through which flyweights can receive and act on extrinsic state.
* **ConcreteFlyweight**
  * implements the Flyweight interface and adds storage for intrinsic state, if any. A ConcreteFlyweight object must be sharable. Any state it stores must be intrinsic; that is, it must be independent of the ConcreteFlyweight object's context.
* **UnsharedConcreteFlyweight**
  * not all Flyweight subclasses need to be shared. The Flyweight interface enables sharing; it doesn't enforce it. It's common for UnsharedConcreteFlyweight objects to have ConcreteFlyweight objects as children at some level in the flyweight object structure.
* **FlyweightFactory**
  * creates and manages flyweight objects.
  * ensures that flyweights are shared properly. When a client requests a flyweight, the FlyweightFactory object supplies an existing instance or creates one, if none exists.

* **Client**
  * maintains a reference to flyweight(s).
  * computes or stores the extrinsic state of flyweight(s).

## C# implementation

```csharp
void DisplayMemory() =>
    Console.WriteLine($"Number of MBs currently thought to be allocated: {GC.GetTotalMemory(false) / (1024 * 1024)}");

var state = new Flyweight(Color.Red, "Red line", 5);
var lines = new List<Line>();

DisplayMemory();
for (var i = 0; i < 1000000; i++)
{
    lines.Add(new Line
    {
        X1 = Random.Shared.Next(100),
        Y1 = Random.Shared.Next(100),
        X2 = Random.Shared.Next(100),
        Y2 = Random.Shared.Next(100),
        State = state
        // State = new Flyweight(Color.Red, "Red line", 5)
    });
}

DisplayMemory();
var line = lines.First();
Console.WriteLine($"First line coordinates: X1={line.X1}, Y1={line.Y1}, X2={line.X2}, Y2={line.Y2}");
Console.WriteLine("First line length: " + Flyweight.CalculateLength(line.X1, line.Y1, line.X2, line.Y2));

struct Line
{
    public int X1 { get; init; }
    public int Y1 { get; init; }
    public int X2 { get; init; }
    public int Y2 { get; init; }
    public Flyweight State { get; init; }
}

class Flyweight
{
    public Color Color { get; }
    public string Description { get; }
    public int Priority { get; }

    public Flyweight(Color color, string description, int priority)
    {
        Color = color;
        Description = description;
        Priority = priority;
    }

    public static double CalculateLength(int x1, int y1, int x2, int y2)
    {
        return Math.Sqrt(Math.Pow(x1 - x2, 2) + Math.Pow(y1 - y2, 2));
    }
}

enum Color
{
    Black = 0,
    Red = 1,
    Green = 2,
    Blue = 3
}
```

Output:

```console
Number of MBs currently thought to be allocated: 1
Number of MBs currently thought to be allocated: 46
First line coordinates: X1=82, Y1=75, X2=50, Y2=93
First line length: 36.71511950137164
```
