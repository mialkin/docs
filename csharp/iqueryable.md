# `IQueryable<T>` interface

```csharp
/// <summary>Provides functionality to evaluate queries against a specific data source wherein the type of the data is known.</summary>
/// <typeparam name="T">The type of the data in the data source.</typeparam>
public interface IQueryable<out T> : IEnumerable<T>, IEnumerable, IQueryable
{
}
```

```csharp
/// <summary>Provides functionality to evaluate queries against a specific data source wherein the type of the data is not specified.</summary>
public interface IQueryable : IEnumerable
{
    /// <summary>Gets the expression tree that is associated with the instance of <see cref="T:System.Linq.IQueryable" />.</summary>
    /// <returns>The <see cref="T:System.Linq.Expressions.Expression" /> that is associated with this instance of <see cref="T:System.Linq.IQueryable" />.</returns>
    Expression Expression { get; }

    /// <summary>Gets the type of the element(s) that are returned when the expression tree associated with this instance of <see cref="T:System.Linq.IQueryable" /> is executed.</summary>
    /// <returns>A <see cref="T:System.Type" /> that represents the type of the element(s) that are returned when the expression tree associated with this object is executed.</returns>
    Type ElementType { get; }

    /// <summary>Gets the query provider that is associated with this data source.</summary>
    /// <returns>The <see cref="T:System.Linq.IQueryProvider" /> that is associated with this data source.</returns>
    IQueryProvider Provider { get; }
}
```

The last property gives us an instance of the interface `IQueryProvider` which contains all the methods that implement constructing new `IQueryable`s and executing them off:

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

This interface inherits the [`IEnumerable<T>`](ienumerable.md) interface so that if it represents a query, the results of that query can be enumerated. Enumeration forces the expression tree associated with an `IQueryable<T>` object to be executed. Queries that do not return enumerable results are executed when the `Execute<TResult>(Expression)` method is called.

The definition of "executing an expression tree" is specific to a query provider. For example, it may involve translating the expression tree to a query language appropriate for an underlying data source.

The `IQueryable<T>` interface enables queries to be polymorphic. That is, because a query against an `IQueryable` data source is represented as an expression tree, it can be executed against different types of data sources.

## Links

[↑ `IQueryable<T>` interface](https://docs.microsoft.com/en-us/dotnet/api/system.linq.iqueryable-1).

[↑ LINQ: Building an IQueryable Provider - Part I](https://docs.microsoft.com/en-us/archive/blogs/mattwar/linq-building-an-iqueryable-provider-part-i).
