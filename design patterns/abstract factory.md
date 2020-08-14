# Abstract factory

The **abstract factory** is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.

## Participants

* **AbstractFactory**
  * declares an interface for operations that create abstract product
objects.
* **ConcreteFactory**
  * implements the operations to create concrete product objects.
* **AbstractProduct**
  * declares an interface for a type of product object.
* **ConcreteProduct**
  * defines a product object to be created by the corresponding concrete
factory.
  * implements the AbstractProduct interface.
* **Client**
  * uses only interfaces declared by AbstractFactory and
AbstractProduct classes.

## C# implementation

```csharp
using System;

class Program
{
    static void Main()
    {
        Client(new FactoryA());
        Client(new FactoryB());
    }

    private static void Client(IFactory factory)
    {
        IProduct product = factory.GetProduct();
        product.Method();
    }
}

interface IProduct
{
    void Method();
}

class ProductA : IProduct
{
    public void Method()
    {
        Console.WriteLine("A");
    }
}

class ProductB : IProduct
{
    public void Method()
    {
        Console.WriteLine("B");
    }
}

interface IFactory
{
    IProduct GetProduct();
}

class FactoryA : IFactory
{
    public IProduct GetProduct()
    {
        return new ProductA();
    }
}

class FactoryB : IFactory
{
    public IProduct GetProduct()
    {
        return new ProductB();
    }
}
```
Output:

```console
A
B
```
