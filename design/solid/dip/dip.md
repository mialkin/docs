# Dependency Inversion Principle

The principle states<sup>1</sup>:

* A. High-level modules should not depend on low-level modules. Both should depend on abstractions.

* B. Abstractions should not depend on details. Details should depend on abstractions.

Simplier definition:

> Depend on abstractions, not on concretions.

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

Diagram that demonstrates dependencies of the above code:

<img src="svg/dip-no.svg">

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

Diagram that demonstrates dependencies of the above code:

<img src="svg/dip.svg">

The direction of dependency within the application should be in the direction of abstraction, not implementation details. Most applications are written such that compile-time dependency flows in the direction of runtime execution, producing a direct dependency graph.

Dependency inversion is a key part of building loosely coupled applications, since implementation details can be written to depend on and implement higher-level abstractions, rather than the other way around. The resulting applications are more testable, modular, and maintainable as a result.

The dependency injection (DI) is a software design pattern, which is a technique for achieving Inversion of Control (IoC) between classes and their dependencies.

<hr>

<sup>1</sup> Robert C. Martin, Agile Software Development, Principles, Patterns, and Practices (Upper Saddle River, NJ: Prentice Hall, 2002), p. 127.
