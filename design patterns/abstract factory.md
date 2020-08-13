# Abstract factory

The **abstract factory** is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.

```csharp
using System;

class Program
{
    static void Main()
    {
        CallProduct(new FactoryA());
        CallProduct(new FactoryB());
    }

    private static void CallProduct(IFactory factory)
    {
        IProduct product = factory.GetProduct();
        product.Method();
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

public interface IFactory
{
    IProduct GetProduct();
}

public class FactoryA : IFactory
{
    public IProduct GetProduct()
    {
        return new ProductA();
    }
}

public class FactoryB : IFactory
{
    public IProduct GetProduct()
    {
        return new ProductB();
    }
}
```
