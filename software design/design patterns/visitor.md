# Visitor

The **Visitor** is a behavioral software design pattern that represents an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.

## Participants

* **Visitor**
  * declares a Visit operation for each class of ConcreteElement in the object structure. The operation's name and signature identifies the class that sends the Visit request to the visitor. That lets the visitor determine the concrete class of the element being visited. Then the visitor can access the element directly through its particular interface.
* **ConcreteVisitor**
  * implements each operation declared by Visitor. Each operation implements a fragment of the algorithm defined for the corresponding class of object in the structure. ConcreteVisitor provides the context for the algorithm and stores its local state. This state often accumulates results during the traversal of the structure.
* **Element**
  * defines an Accept operation that takes a visitor as an argument.
* **ConcreteElement**
  * implements an Accept operation that takes a visitor as an argument.
* **ObjectStructure**
  * can enumerate its elements.
  * may provide a high-level interface to allow the visitor to visit its elements.
  * may either be a composite or a collection such as a list or a set.

## C# implementation

```csharp
using System;
using System.Collections.Generic;

class Program
{
    private static readonly IVisitor VisitorOne = new VisitorOne();
    private static readonly IVisitor VisitorTwo = new VisitorTwo();

    public static void Main()
    {
        IElement element = new ElementB(8);
        element.AddChild(new ElementB(12));
        element.AddChild(new ElementA(5));
        element.AddChild(new ElementA(50));

        IElement root = new ElementA(20);
        root.AddChild(element);
        root.AddChild(new ElementA(7));

        Traverse(root, 0);
    }


    private static void Traverse(IElement element, int padding)
    {
        Console.Write(new String('\t', padding++));

        element.Accept(element.Value < 10 ? VisitorOne : VisitorTwo);

        foreach (IElement child in element.Children)
        {
            Traverse(child, padding);
        }
    }
}

interface IVisitor
{
    void Visit(ElementA element);
    void Visit(ElementB element);
}

class VisitorOne : IVisitor
{
    public void Visit(ElementA element)
    {
        Console.WriteLine($"Element A{element.Value} processed by VisitorOne");
    }

    public void Visit(ElementB element)
    {
        Console.WriteLine($"Element B{element.Value} processed by VisitorOne");
    }
}

class VisitorTwo : IVisitor
{
    public void Visit(ElementA element)
    {
        Console.WriteLine($"Element A{element.Value} processed by VisitorTwo");
    }

    public void Visit(ElementB element)
    {
        Console.WriteLine($"Element B{element.Value} processed by VisitorTwo");
    }
}

interface IElement
{
    void Accept(IVisitor visitor);

    public int Value { get; }

    List<IElement> Children { get; }

    void AddChild(IElement child);
}

class ElementA : IElement
{
    public int Value { get; }
    public List<IElement> Children { get; } = new List<IElement>();

    public ElementA(int value)
    {
        Value = value;
    }

    public void AddChild(IElement child)
    {
        Children.Add(child);
    }

    public void Accept(IVisitor visitor)
    {
        visitor.Visit(this);
    }
}

class ElementB : IElement
{
    public int Value { get; }
    public List<IElement> Children { get; } = new List<IElement>();

    public ElementB(int value)
    {
        Value = value;
    }

    public void AddChild(IElement child)
    {
        Children.Add(child);
    }

    public void Accept(IVisitor visitor)
    {
        visitor.Visit(this);
    }
}
```

Output:

```output
Element A20 processed by VisitorTwo
        Element B8 processed by VisitorOne
                Element B12 processed by VisitorTwo
                Element A5 processed by VisitorOne
                Element A50 processed by VisitorTwo
        Element A7 processed by VisitorOne
```
