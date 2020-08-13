# Abstract factory

```cs
using System;

class Program
{
    static void Main()
    {
        CallProductMethod(new FactoryA());
        CallProductMethod(new FactoryB());
    }

    private static void CallProductMethod(IFactory factory)
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
