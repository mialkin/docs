# xUnit

## TOC

- [xUnit](#xunit)
  - [TOC](#toc)
  - [`Theory`](#theory)
  - [Setup And Teardown](#setup-and-teardown)

## `Theory`

`InlineData` attribute:

```csharp
[Theory]
[InlineData(3)]
[InlineData(5)]
public void Number_Is_Odd(int number)
{
    Assert.True(number % 2 == 1);
}
```

`ClassData` attribute:

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

## Setup And Teardown

> While text below is working look at unit tests best practice that says "Prefer helper methods to setup and teardown"

xUnit creates a new instance of the test class for every test that is run. This makes the constructor a convenient place to put reusable context setup code.

For context cleanup, add the `IDisposable` interface to your test class, and put the cleanup code in the `Dispose()` method.
