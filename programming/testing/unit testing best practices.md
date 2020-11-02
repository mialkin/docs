# Unit testing best practices

A **fake** is a generic term that can be used to describe either a *stub* or a *mock* object. Whether it's a *stub* or a *mock* depends on the context in which it's used.

A **stub** is a controllable replacement for an existing dependency (or collaborator) in the system. By using a stub, you can test your code without dealing with the dependency directly. By default, a *fake* starts out as a *stub*.

Stub example:

```csharp
var stubOrder = new FakeOrder();
var purchase = new Purchase(stubOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

A **mock** is a fake object in the system that decides whether or not a unit test has passed or failed. A mock starts out as a *fake* until it's asserted against.

Mock example:

```csharp
var mockOrder = new FakeOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(mockOrder.Validated);
```

The main thing to remember about mocks versus stubs is that mocks are just like stubs, but you assert against the mock object, whereas you do not assert against a stub.

## Links

[↑ Unit testing best practices with .NET Core and .NET Standard](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices)
