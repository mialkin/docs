# Factory Method

The **Factory Method** is creational software design pattern that defines an interface for creating an object, but lets subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

## Participants

* **Product**
  * defines the interface of objects the factory method creates.
* **ConcreteProduct**
  * implements the Product interface.
* **Creator**
  * declares the factory method, which returns an object of type Product. Creator may also define a default implementation of the factory method that returns a default ConcreteProduct object.
  * may call the factory method to create a Product object.
* **ConcreteCreator**
  * overrides the factory method to return an instance of a ConcreteProduct.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        Client(new CreatorA());
        Client(new CreatorB());
        Client(new CreatorC());
    }

    static void Client(ICreator creator)
    {
        IProduct product = creator.GetProduct();
        product.Act();
    }
}

interface ICreator
{
    IProduct GetProduct()
    {
        return new DefaultProduct();
    }
}

class CreatorA : ICreator
{
    public virtual IProduct GetProduct()
    {
        return new ProductA();
    }
}

class CreatorB : CreatorA
{
    public override IProduct GetProduct()
    {
        return new ProductB();
    }
}

class CreatorC : ICreator
{
}

interface IProduct
{
    void Act();
}

class ProductA : IProduct
{
    public void Act()
    {
        Console.WriteLine("A");
    }
}

class ProductB : IProduct
{
    public void Act()
    {
        Console.WriteLine("B");
    }
}

class DefaultProduct : IProduct
{
    public void Act()
    {
        Console.WriteLine("Default");
    }
}
```

Output:

```output
A
B
Default
```
