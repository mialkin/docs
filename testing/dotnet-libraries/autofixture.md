# AutoFixture

[↑ AutoFixture](https://github.com/AutoFixture/AutoFixture) is a .NET library that makes it easier for developers to do test-driven development by automating non-relevant test fixture setup, allowing the test developer to focus on the essentials of each test case.

When writing unit tests, you typically need to create some objects that represent the initial state of the test. Often, an API will force you to specify much more data than you really care about, so you frequently end up creating objects that has no influence on the test, simply to make the code compile. AutoFixture can create values of virtually any type without the need for you to explicitly define which values should be used.

AutoFixture will by default, take care of providing data to the constructor  of the class, and will fill in the data of all public properties.

## Installation

```bash
dotnet add package AutoFixture.Xunit2
```

## Links

- [↑ Cheat sheet](https://github.com/AutoFixture/AutoFixture/wiki/Cheat-Sheet)
- [↑ AutoMoq explanation](https://autofixture.github.io/docs/quick-start)