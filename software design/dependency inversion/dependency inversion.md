# Dependency Inversion Principle

 The principle states<sup>1</sup>:

a. High-level modules should not depend on low-level modules. Both should depend on abstractions.

b. Abstractions should not depend on details. Details should depend on abstractions.

## C# example without dependency inversion

```csharp
using System;

class Program
{
    public static void Main()
    {
        var service = new Service();
        Run(service);
    }

    private static void Run(Service service)
    {
        Console.WriteLine(service.Calculate());
    }
}

class Service
{
    private readonly Repository _repository;

    public Service()
    {
        _repository = new Repository();
    }

    public int Calculate()
    {
        int data = _repository.GetData();

        return data * 5;
    }
}

class Repository
{
    public int GetData()
    {
        return 40;
    }
}
```

Output:

```output
200
```

## Diagram that demonstrates dependencies of the above code

<img src="dependency inversion no.svg">

## C# example that takes into account the Dependency Inversion Principle

```csharp
using System;

class Program
{
    public static void Main()
    {
        IService service = new Service(new Repository());
        Run(service);
    }

    private static void Run(IService service)
    {
        Console.WriteLine(service.Calculate());
    }
}

interface IService
{
    int Calculate();
}

class Service : IService
{
    private readonly IRepository _repository;

    public Service(IRepository repository)
    {
        _repository = repository;
    }

    public int Calculate()
    {
        int data = _repository.GetData();

        return data * 5;
    }
}

interface IRepository
{
    int GetData();
}

class Repository : IRepository
{
    public int GetData()
    {
        return 40;
    }
}
```

Output:

```output
200
```

## Diagram that demonstrates dependencies of the above code

<img src="dependency inversion.svg">

<hr>

<sup>1</sup> Robert C. Martin, Agile Software Development, Principles, Patterns, and Practices
(Upper Saddle River, NJ: Prentice Hall, 2002), pp. 127.
