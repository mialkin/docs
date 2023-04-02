# Moq

[↑ Moq](https://github.com/moq/moq4) is a mocking library for .NET.

```bash
dotnet add package Moq
```

## Table of Contents

- [Moq](#moq)
  - [Table of Contents](#table-of-contents)
  - [Creating mock object](#creating-mock-object)
  - [Matching arguments](#matching-arguments)
    - [Returning value that was passed into a method](#returning-value-that-was-passed-into-a-method)
    - [`It.IsAny<>`](#itisany)
    - [`It.Is`](#itis)
    - [Defined variable](#defined-variable)
  - [Links](#links)

## Creating mock object

```csharp
 var mock = new Mock<ILoveThisLibrary>();
```

## Matching arguments

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

### Returning value that was passed into a method

```csharp
mock.Setup(x => x.Sum(It.IsAny<int>(), It.IsAny<int>()))
    .Returns((int a, int b) => a - b);

var calculator = mock.Object;
var sum = calculator.Sum(10, 4); // 6
```

### `It.IsAny<>`

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

### `It.Is`

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

### Defined variable

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

## Links

[↑ Quick start guide](https://github.com/Moq/moq4/wiki/Quickstart)