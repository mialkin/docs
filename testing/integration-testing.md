# Integration testing

Integration tests ensure that an application's components function correctly at a level that includes the application's supporting infrastructure, such as the database, file system, and network.

ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.

[↑ Integration tests in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/test/integration-tests)

## Introduction to integration tests

Integration tests evaluate an application's components on a broader level than unit tests. Unit tests are used to test isolated software components, such as individual class methods. Integration tests confirm that two or more application components work together to produce an expected result, possibly including every component required to fully process a request.

These broader tests are used to test the application's infrastructure and whole framework, often including the following components:

- Database
- File system
- Network appliances
- Request-response pipeline

Unit tests use fabricated components, known as fakes or mock objects, in place of infrastructure components.

In contrast to unit tests, integration tests:

- Use the actual components that the application uses in production
- Require more code and data processing
- Take longer to run
  
Therefore, limit the use of integration tests to the most important infrastructure scenarios. If a behavior can be tested using either a unit test or an integration test, choose the unit test.

## ASP.NET Core integration tests

Integration tests in ASP.NET Core require the following:

- A test project is used to contain and execute the tests. The test project has a reference to the SUT.
- The test project creates a test web host for the SUT and uses a test server client to handle requests and responses with the SUT.
- A test runner is used to execute the tests and report the test results.

Integration tests follow a sequence of events that include the usual *arrange*, *act*, and *assert* test steps:

1. The SUT's web host is configured
2. A test server client is created to submit requests to the application
3. The arrange test step is executed: the test application prepares a request
4. The act test step is executed: the client submits the request and receives the response
5. The assert test step is executed: the actual response is validated as a pass or fail based on an expected response
6. The process continues until all of the tests are executed
7. The test results are reported

Usually, the test web host is configured differently than the application's normal web host for the test runs. For example, a different database or different application settings might be used for the tests.

Infrastructure components, such as the test web host and in-memory test server, are provided or managed by the [↑ Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package. Use of this package streamlines test creation and execution.

> When creating a test project for an application, separate the unit tests from the integration tests into different projects. This helps ensure that infrastructure testing components aren't accidentally included in the unit tests. Separation of unit and integration tests also allows control over which set of tests are run
