# Composite

The **Composite** is a structural software desing pattern that composes objects into tree structures to represent part-whole hierarchies.
Composite lets clients treat individual objects and compositions of objects uniformly.

## Participants

* **Component**
  * declares the interface for objects in the composition.
  * implements default behavior for the interface common to all classes,
as appropriate.
  * declares an interface for accessing and managing its child
components.
  * (optional) defines an interface for accessing a component's parent
in the recursive structure, and implements it if that's appropriate.
* **Leaf**
  * represents leaf objects in the composition. A leaf has no children.
  * defines behavior for primitive objects in the composition.
* **Composite**
  * defines behavior for components having children.
  * stores child components.
  * implements child-related operations in the Component interface.
* **Client**
  * manipulates objects in the composition through the Component
interface.

## C# implementation

```csharp
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    public static void Main()
    {
        var c1 = new Composite(1);
        c1.Add(new Leaf(2));
        c1.Add(new Leaf(3));

        var c2 = new Composite(4);
        c2.Add(new Leaf(5));
        c2.Add(new Composite(6));

        var c3 = new Composite(7);
        c3.Add(new Leaf(8));

        c2.Add(c3);
        c2.Add(new Leaf(9));

        c1.Add(c2);
        c1.Add(new Leaf(10));

        Client(c1);
    }

    static void Client(IComponent component)
    {
        ExecuteRecursively(component, 0);
    }

    static void ExecuteRecursively(IComponent component, int padding)
    {
        Console.Write(new String('\t', padding));
        component.Act();

        if (component.Children == null)
            return;

        padding++;

        foreach (IComponent child in component.Children)
        {
            Console.Write(new String('\t', padding));
            ExecuteRecursively(child, padding);
        }
    }
}

interface IComponent
{
    int Id { get; }
    IComponent Parent { get; set; }
    IList<IComponent> Children { get; }

    void Add(IComponent component);
    void Remove(IComponent component);

    void Act();
}

class Composite : IComponent
{
    public int Id { get; }

    public IComponent Parent { get; set; }

    public IList<IComponent> Children { get; }

    public Composite(int id)
    {
        Id = id;
        Children = new List<IComponent>();
    }

    public void Add(IComponent component)
    {
        var item = Children.SingleOrDefault(x => x.Id == component.Id);
        if (item == null)
        {
            component.Parent = this;
            Children.Add(component);
        }
        else
        {
            throw new Exception("Can't add second component with the same ID");
        }
    }

    public void Remove(IComponent component)
    {
        var item = Children.SingleOrDefault(x => x.Id == component.Id);
        if (item != null)
            Children.Remove(item);
    }

    public void Act()
    {
        Console.WriteLine($"Composite. {Id}");
    }
}

class Leaf : IComponent
{
    public int Id { get; }

    public IComponent Parent { get; set; }

    public IList<IComponent> Children => null;

    public Leaf(int id)
    {
        Id = id;
    }

    public void Add(IComponent component)
    {
        throw new Exception("Can't add component to a leaf");
    }

    public void Remove(IComponent component)
    {
        throw new Exception("Can't remove component from a leaf");
    }

    public void Act()
    {
        Console.WriteLine($"Leaf. {Id}");
    }
}
```

Output:

```output
Composite. 1
                Leaf. 2
                Leaf. 3
                Composite. 4
                                Leaf. 5
                                Composite. 6
                                Composite. 7
                                                Leaf. 8
                                Leaf. 9
                Leaf. 10
```
