# Unit testing best practices

- [Unit testing best practices](#unit-testing-best-practices)
  - [Let's speak the same language](#lets-speak-the-same-language)
  - [Naming your tests](#naming-your-tests)
  - [Prefer helper methods to setup and teardown](#prefer-helper-methods-to-setup-and-teardown)
    - [Why?](#why)
    - [Bad](#bad)
    - [Better](#better)
  - [Validate private methods by unit testing public methods](#validate-private-methods-by-unit-testing-public-methods)
  - [Avoid logic in tests](#avoid-logic-in-tests)
    - [Why?](#why-1)
  - [Avoid multiple asserts](#avoid-multiple-asserts)
    - [Why?](#why-2)
  - [Write minimally passing tests](#write-minimally-passing-tests)
    - [Why?](#why-3)
  - [Code coverage](#code-coverage)
  - [Links](#links)

## Let's speak the same language

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

## Naming your tests

The name of your test should consist of three parts:

- The name of the method being tested.
- The scenario under which it's being tested.
- The expected behavior when the scenario is invoked.

Example:

```csharp
[Fact]
public void Add_SingleNumber_ReturnsSameNumber()
{
    // Arrange
    var stringCalculator = new StringCalculator();

    // Act
    var actual = stringCalculator.Add("0");

    // Assert
    Assert.Equal(0, actual);
}
```

## Prefer helper methods to setup and teardown

If you require a similar object or state for your tests, prefer a helper method than leveraging `Setup` and `Teardown` attributes if they exist.

### Why?

- Less confusion when reading the tests since all of the code is visible from within each test.
- Less chance of setting up too much or too little for the given test.
- Less chance of sharing state between tests, which creates unwanted dependencies between them.

In unit testing frameworks, `Setup` is called before each and every unit test within your test suite. While some may see this as a useful tool, it generally ends up leading to bloated and hard to read tests. Each test will generally have different requirements in order to get the test up and running. Unfortunately, `Setup` forces you to use the exact same requirements for each test.

> xUnit has removed both SetUp and TearDown as of version 2.x

### Bad

```csharp
private readonly StringCalculator stringCalculator;
public StringCalculatorTests()
{
    stringCalculator = new StringCalculator();
}
```

```csharp
[Fact]
public void Add_TwoNumbers_ReturnsSumOfNumbers()
{
    var result = stringCalculator.Add("0,1");

    Assert.Equal(1, result);
}
```

### Better

```csharp
[Fact]
public void Add_TwoNumbers_ReturnsSumOfNumbers()
{
    var stringCalculator = CreateDefaultStringCalculator();

    var actual = stringCalculator.Add("0,1");

    Assert.Equal(1, actual);
}
```

```csharp
private StringCalculator CreateDefaultStringCalculator()
{
    return new StringCalculator();
}
```

## Validate private methods by unit testing public methods

In most cases, there should not be a need to test a private method. Private methods are an implementation detail. You can think of it this way: private methods never exist in isolation. At some point, there is going to be a public facing method that calls the private method as part of its implementation. What you should care about is the end result of the public method that calls into the private one.

Links:

- [Should I test private methods or only public ones?](https://stackoverflow.com/questions/105007/should-i-test-private-methods-or-only-public-ones)
- [How do you unit test private methods?](https://softwareengineering.stackexchange.com/questions/100959/how-do-you-unit-test-private-methods)

## Avoid logic in tests

When writing your unit tests avoid manual string concatenation and logical conditions such as `if`, `while`, `for`, `switch`, etc.

### Why?

- Less chance to introduce a bug inside of your tests.
- Focus on the end result, rather than implementation details.

When you introduce logic into your test suite, the chance of introducing a bug into it increases dramatically. The last place that you want to find a bug is within your test suite. You should have a high level of confidence that your tests work, otherwise, you will not trust them. Tests that you do not trust, do not provide any value. When a test fails, you want to have a sense that something is actually wrong with your code and that it cannot be ignored.

If logic in your test seems unavoidable, consider splitting the test up into two or more different tests.

## Avoid multiple asserts

When writing your tests, try to only include one `Assert` per test. Common approaches to using only one assert include:

- Create a separate test for each assert.
- Use parameterized tests.

### Why?

- If one `Assert` fails, the subsequent `Assert`s will not be evaluated.
- Ensures you are not asserting multiple cases in your tests.
- Gives you the entire picture as to why your tests are failing.

When introducing multiple asserts into a test case, it is not guaranteed that all of the asserts will be executed. In most unit testing frameworks, once an assertion fails in a unit test, the proceeding tests are automatically considered to be failing. This can be confusing as functionality that is actually working, will be shown as failing.

A common exception to this rule is when asserting against an object. In this case, it is generally acceptable to have multiple asserts against each property to ensure the object is in the state that you expect it to be in.

## Write minimally passing tests

The input to be used in a unit test should be the simplest possible in order to verify the behavior that you are currently testing.

### Why?

- Tests become more resilient to future changes in the codebase.
- Closer to testing behavior over implementation.

Tests that include more information than required to pass the test have a higher chance of introducing errors into the test and can make the intent of the test less clear. When writing tests, you want to focus on the behavior. Setting extra properties on models or using non-zero values when not required, only detracts from what you are trying to prove.

## Code coverage

A high code coverage percentage is often associated with a higher quality of code. However, the measurement itself *cannot* determine the quality of code. Setting an overly ambitious code coverage percentage goal can be counterproductive.

A high code coverage percentage is not an indicator of success, nor does it imply high code quality. It just represents the amount of code that is covered by unit tests.

## Links

[↑ Unit testing best practices with .NET Core and .NET Standard](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices)
