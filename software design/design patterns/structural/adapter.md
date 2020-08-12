# Adapter

The **adapter** is a structural software design pattern that converts the interface of a class into another interface clients expect. An adapter lets classes work together that could not otherwise because of incompatible interfaces.

```csharp
using System;

class Program
{
    static void Main()
    {
        IContractB adapter = new Adapter(new A());
        adapter.MethodB();
    }
}

interface IContractA
{
    void MethodA();
}

interface IContractB
{
    void MethodB();
}

class A : IContractA
{
    public void MethodA()
    {
        Console.WriteLine("A");
    }
}

class Adapter : IContractB
{
    private IContractA _contractA;

    public Adapter(IContractA contractA)
    {
        _contractA = contractA;
    }

    public void MethodB()
    {
        _contractA.MethodA();
    }
}
```

Output:

```console
One
```
