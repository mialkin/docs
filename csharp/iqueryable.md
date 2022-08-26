# IQueryable\<T> Interface

Provides functionality to evaluate queries against a specific data source wherein the type of the data is known.

If you use Visual Studio to go to definition, you get something that looks like this:

```csharp
public interface IQueryable : IEnumerable
{
    Type ElementType { get; }
    Expression Expression { get; }
    IQueryProvider Provider { get; }
}

public interface IQueryable<T> : IEnumerable<T>, IQueryable, IEnumerable
{
}
```

The last property gives us an instance of the interface `IQueryProvider` which contains all the methods that implement constructing new `IQueryables` and executing them off:

```csharp
public interface IQueryProvider
{
    IQueryable CreateQuery(Expression expression);

    IQueryable<TElement> CreateQuery<TElement>(Expression expression);

    object Execute(Expression expression);

    TResult Execute<TResult>(Expression expression);
}
```

## Remarks

The `IQueryable<T>` interface is intended for implementation by query providers.

This interface inherits the [IEnumerable<T>](ienumerable.md) interface so that if it represents a query, the results of that query can be enumerated. Enumeration forces the expression tree associated with an `IQueryable<T>` object to be executed. Queries that do not return enumerable results are executed when the `Execute<TResult>(Expression)` method is called.

The definition of "executing an expression tree" is specific to a query provider. For example, it may involve translating the expression tree to a query language appropriate for an underlying data source.

The `IQueryable<T>` interface enables queries to be polymorphic. That is, because a query against an `IQueryable` data source is represented as an expression tree, it can be executed against different types of data sources.

## Links

* [↑ IQueryable\<T> Interface](https://docs.microsoft.com/en-us/dotnet/api/system.linq.iqueryable-1)
* [↑ LINQ: Building an IQueryable Provider - Part I](https://docs.microsoft.com/en-us/archive/blogs/mattwar/linq-building-an-iqueryable-provider-part-i)