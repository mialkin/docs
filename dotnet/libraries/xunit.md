# xUnit

[↑ xUnit.net](https://github.com/xunit/xunit) is a free, open source, community-focused unit testing tool for the .NET.

## Table of contents

- [xUnit](#xunit)
  - [Table of contents](#table-of-contents)
  - [`InlineData`](#inlinedata)
  - [`ClassData`](#classdata)
  - [Articles](#articles)
  - [Setup and teardown](#setup-and-teardown)

## `InlineData`

```csharp
[Theory]
[InlineData(3)]
[InlineData(5)]
public void Number_Is_Odd(int number)
{
    Assert.True(number % 2 == 1);
}
```

## `ClassData`

```csharp
public class ExampleTest
{
    [Theory]
    [ClassData(typeof(ExampleData))]
    public void Number_Is_Null_Or_Zero(byte? number)
    {
        Assert.True(number == null || number == 0);
    }

    private class ExampleData : TheoryData<byte?>
    {
        public ExampleData()
        {
            Add(null);
            Add(0);
        }
    }
}
```

## Articles

[↑ Creating strongly typed xUnit theory test data with TheoryData](https://andrewlock.net/creating-strongly-typed-xunit-theory-test-data-with-theorydata/)

[↑ Creating parameterized tests in xUnit with `[InlineData]`, `[ClassData]`, and `[MemberData]`](https://andrewlock.net/creating-parameterised-tests-in-xunit-with-inlinedata-classdata-and-memberdata/)

## Setup and teardown

> While text below is working look at unit tests best practice that says "Prefer helper methods to setup and teardown"

xUnit creates a new instance of the test class for every test that is run. This makes the constructor a convenient place to put reusable context setup code.

For context cleanup, add the `IDisposable` interface to your test class, and put the cleanup code in the `Dispose()` method.
