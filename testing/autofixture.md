# AutoFixture, Fluent Assertions, Moq, xUnit

## Table of contents

- [AutoFixture, Fluent Assertions, Moq, xUnit](#autofixture-fluent-assertions-moq-xunit)
  - [Table of contents](#table-of-contents)
  - [AutoFixture](#autofixture)
    - [Installation](#installation)
    - [.NET repository](#net-repository)
    - [Links](#links)
  - [Fluent Assertions](#fluent-assertions)
    - [`async` method](#async-method)
  - [Moq](#moq)
    - [Creating mock object](#creating-mock-object)
    - [Matching arguments](#matching-arguments)
      - [Returning value that was passed into a method](#returning-value-that-was-passed-into-a-method)
      - [`It.IsAny<>`](#itisany)
      - [`It.Is`](#itis)
      - [Defined variable](#defined-variable)
    - [Links](#links-1)
  - [xUnit](#xunit)
    - [Table of contents](#table-of-contents-1)
    - [`InlineData`](#inlinedata)
    - [`ClassData`](#classdata)
    - [Articles](#articles)
    - [Setup and teardown](#setup-and-teardown)

## AutoFixture

[↑ AutoFixture](https://github.com/AutoFixture/AutoFixture) is a .NET library that makes it easier for developers to do test-driven development by automating non-relevant test fixture setup, allowing the test developer to focus on the essentials of each test case.

When writing unit tests, you typically need to create some objects that represent the initial state of the test. Often, an API will force you to specify much more data than you really care about, so you frequently end up creating objects that has no influence on the test, simply to make the code compile. AutoFixture can create values of virtually any type without the need for you to explicitly define which values should be used.

AutoFixture will by default, take care of providing data to the constructor  of the class, and will fill in the data of all public properties.

### Installation

```bash
dotnet add package AutoFixture.Xunit2
```

### .NET repository

[↑ tests](https://github.com/mialkin/tests).

### Links

- [↑ Cheat sheet](https://github.com/AutoFixture/AutoFixture/wiki/Cheat-Sheet)
- [↑ AutoMoq explanation](https://autofixture.github.io/docs/quick-start)
- [↑ Write simpler tests with Type Builders and AutoFixture](https://canro91.github.io/2021/06/21/WriteSimplerTestsTypeBuilderAndAutoFixture)

## Fluent Assertions

[↑ Fluent Assertions](https://fluentassertions.com/introduction) is a set of .NET extension methods that allows to specify the expected outcome of a unit test.

### `async` method

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

## Moq

[↑ Moq](https://github.com/moq/moq4) is a mocking library for .NET.

```bash
dotnet add package Moq
```

### Creating mock object

```csharp
 var mock = new Mock<ILoveThisLibrary>();
```

### Matching arguments

```csharp
// any value
mock.Setup(foo => foo.DoSomething(It.IsAny<string>())).Returns(true);


// matching Func<int>, lazy evaluated
mock.Setup(foo => foo.Add(It.Is<int>(i => i % 2 == 0))).Returns(true); 


// matching ranges
mock.Setup(foo => foo.Add(It.IsInRange<int>(0, 10, Range.Inclusive))).Returns(true); 


// matching regex
mock.Setup(x => x.DoSomething(It.IsRegex("[a-d]+", RegexOptions.IgnoreCase))).Returns("foo");
```

#### Returning value that was passed into a method

```csharp
mock.Setup(x => x.Sum(It.IsAny<int>(), It.IsAny<int>()))
    .Returns((int a, int b) => a - b);

var calculator = mock.Object;
var sum = calculator.Sum(10, 4); // 6
```

#### `It.IsAny<>`

The method or property set up with `It.IsAny<>` means that a call to that method/property with any parameters will not fail and will return a default value for the particular return type:

```csharp
[Fact]
public void Get_WhenUserExists_ReturnsUser_IsAny()
{
    // Arrange.
    var userId = 25;
    var userRepository = new Mock<IUserRepository>();
    userRepository
        .Setup(x => x.Get(It.IsAny<int>()))
        .Returns(new User(userId, It.IsAny<string>(), It.IsAny<DateTime>()));

    var sut = new UserService(userRepository.Object);

    // Act.
    var user = sut.Get(userId);

    // Assert.
    user!.Id.Should().Be(userId);
}
```

#### `It.Is`

The `It.Is<>` construct is useful for setup or verification of a method, letting to specify a function that will match the argument:

```csharp
[Fact]
public void Get_WhenUserExists_ReturnsUser_Is()
{
    // Arrange.
    var userId = 25;
    var userRepository = new Mock<IUserRepository>();
    userRepository
        .Setup(x => x.Get(It.Is<int>(y => y > 20)))
        .Returns(new User(userId, It.IsAny<string>(), It.IsAny<DateTime>()));

    var sut = new UserService(userRepository.Object);

    // Act.
    var user = sut.Get(userId);

    // Assert.
    user!.Id.Should().Be(userId);
}
```

#### Defined variable

When you set up or verify a method parameter with a variable, you're saying you want exactly that value. A method called with another value will never match your setup/verify:

```csharp
[Fact]
public void Get_WhenUserExists_ReturnsUser_Variable()
{
    // Arrange.
    var userId = 25;
    var userRepository = new Mock<IUserRepository>();
    userRepository
        .Setup(x => x.Get(25))
        .Returns(new User(userId, It.IsAny<string>(), It.IsAny<DateTime>()));

    var sut = new UserService(userRepository.Object);

    // Act.
    var user = sut.Get(userId);

    // Assert.
    user!.Id.Should().Be(userId);
}
```

### Links

[↑ Quick start guide](https://github.com/Moq/moq4/wiki/Quickstart)

## xUnit

[↑ xUnit.net](https://github.com/xunit/xunit) is a free, open source, community-focused unit testing tool for the .NET.

### Table of contents

- [xUnit](#xunit)
  - [Table of contents](#table-of-contents)
  - [`InlineData`](#inlinedata)
  - [`ClassData`](#classdata)
  - [Articles](#articles)
  - [Setup and teardown](#setup-and-teardown)

### `InlineData`

```csharp
[Theory]
[InlineData(3)]
[InlineData(5)]
public void Number_Is_Odd(int number)
{
    Assert.True(number % 2 == 1);
}
```

### `ClassData`

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

### Articles

[↑ Creating strongly typed xUnit theory test data with TheoryData](https://andrewlock.net/creating-strongly-typed-xunit-theory-test-data-with-theorydata/)

[↑ Creating parameterized tests in xUnit with `[InlineData]`, `[ClassData]`, and `[MemberData]`](https://andrewlock.net/creating-parameterised-tests-in-xunit-with-inlinedata-classdata-and-memberdata/)

### Setup and teardown

> While text below is working look at unit tests best practice that says "Prefer helper methods to setup and teardown"

xUnit creates a new instance of the test class for every test that is run. This makes the constructor a convenient place to put reusable context setup code.

For context cleanup, add the `IDisposable` interface to your test class, and put the cleanup code in the `Dispose()` method.
