# Factory method

The **factory method** is creational design pattern that defines an interface for creating an object, but lets subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

## Participants

* **Product**
  * defines the interface of objects the factory method creates.
* **ConcreteProduct**
  * implements the Product interface.
* **Creator**
  * declares the factory method, which returns an object of type Product. Creator may also define a default implementation of the factory method that returns a default ConcreteProduct object.
  * may call the factory method to create a Product object.
* **ConcreteCreator**
  * overrides the factory method to return an instance of a
ConcreteProduct.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        IProduct product = GetProduct(new CreatorA());
        product.Method();
        
        product = GetProduct(new CreatorB());
        product.Method();
    }

    static IProduct GetProduct(Creator creator)
    {
        return creator.GetProduct();
    }
}

abstract class Creator
{
    public virtual IProduct GetProduct()
    {
        return new DefaultProduct();
    }
}

class CreatorA : Creator
{
    public override IProduct GetProduct()
    {
        return new ProductA();
    }
}

class CreatorB : Creator
{
    public override IProduct GetProduct()
    {
        return new ProductB();
    }
}

public interface IProduct
{
    void Method();
}

public class ProductA : IProduct
{
    public void Method()
    {
        Console.WriteLine("A");
    }
}

public class ProductB : IProduct
{
    public void Method()
    {
        Console.WriteLine("B");
    }
}

public class DefaultProduct : IProduct
{
    public void Method()
    {
        Console.WriteLine("Default");
    }
}
```

Output:

```console
A
B
```
