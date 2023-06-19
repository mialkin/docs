# Fluent Assertions

[↑ Fluent Assertions](https://fluentassertions.com/introduction) is a set of .NET extension methods that allows to specify the expected outcome of a unit test.

## `async` method

```csharp
// Arrange
var worker = new Worker();

// Act
var func = async () => await worker.DoWorkAsync(0);

// Assert
await func.Should()
    .ThrowAsync<ArgumentOutOfRangeException>()
    .WithMessage("Counter must be positive (Parameter 'counter')");
```
