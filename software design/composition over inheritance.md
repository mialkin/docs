# Composition over inheritance

 The **composition over inheritance** is the principle that classes should achieve polymorphic behavior and code reuse by their composition rather than inheritance from a base or parent class.

An implementation of composition over inheritance typically begins with the creation of various interfaces representing the behaviors that the system must exhibit. Interfaces enable polymorphic behavior. Thus, system behaviors are realized without inheritance.

> Favor object composition over class inheritance.
> –– <cite>[Design Patterns: Elements of Reusable Object-Oriented Software (1994)][1]</cite>

[1]: https://en.wikipedia.org/wiki/Design_Patterns

The following C# example demonstrates the principle of using composition and interfaces to achieve code reuse and polymorphism.

```csharp
using System;

class Program
{
    static void Main()
    {
        var player = new GameObject(new Visible(), new Movable(), new Solid());
        player.Update(); player.Collide(); player.Draw(); Console.WriteLine();

        var cloud = new GameObject(new Visible(), new Movable(), new NotSolid());
        cloud.Update(); cloud.Collide(); cloud.Draw(); Console.WriteLine();

        var building = new GameObject(new Visible(), new NotMovable(), new Solid());
        building.Update(); building.Collide(); building.Draw(); Console.WriteLine();

        var trap = new GameObject(new Invisible(), new NotMovable(), new Solid());
        trap.Update(); trap.Collide(); trap.Draw(); Console.WriteLine();
    }
}

interface IVisible
{
    void Draw();
}

class Invisible : IVisible
{
    public void Draw()
    {
        Console.WriteLine("I won't appear.");
    }
}

class Visible : IVisible
{
    public void Draw()
    {
        Console.WriteLine("I'm showing myself.");
    }
}

interface ICollidable
{
    void Collide();
}

class Solid : ICollidable
{
    public void Collide()
    {
        Console.WriteLine("Bang!");
    }
}

class NotSolid : ICollidable
{
    public void Collide()
    {
        Console.WriteLine("Splash!");
    }
}

interface IUpdatable
{
    void Update();
}

class Movable : IUpdatable
{
    public void Update()
    {
        Console.WriteLine("Moving forward.");
    }
}

class NotMovable : IUpdatable
{
    public void Update()
    {
        Console.WriteLine("I'm staying put.");
    }
}

class GameObject : IVisible, IUpdatable, ICollidable
{
    private readonly IVisible _visible;
    private readonly IUpdatable _updatable;
    private readonly ICollidable _collidable;
    public GameObject(IVisible visible, IUpdatable updatable, ICollidable collidable)
    {
        _visible = visible;
        _updatable = updatable;
        _collidable = collidable;
    }

    public void Update()
    {
        _updatable.Update();
    }

    public void Draw()
    {
        _visible.Draw();
    }

    public void Collide()
    {
        _collidable.Collide();
    }
}

```

Output:

```console
Moving forward.
Bang!
I'm showing myself.

Moving forward.
Splash!
I'm showing myself.

I'm staying put.
Bang!
I'm showing myself.

I'm staying put.
Bang!
I won't appear.
```

## See also

[Strategy pattern](design%20patterns/behavioral/strategy.md)
