# Moq

Moq is a mocking library for .NET.

## Table of Contents

- [Moq](#moq)
  - [Table of Contents](#table-of-contents)
  - [`It.IsAny<>`](#itisany)
  - [`It.Is`](#itis)
  - [Defined variable](#defined-variable)

## `It.IsAny<>`

The method set up with `It.IsAny<>` will match any parameter given to the method:

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

## `It.Is`

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

## Defined variable

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
