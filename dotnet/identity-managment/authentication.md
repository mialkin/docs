# Authentication

Authentication is the process of determining user's identity. Authentication may create *one or more identities* for the current user.

## Table of contents

- [Authentication](#authentication)
  - [Table of contents](#table-of-contents)
  - [Authentication service](#authentication-service)
  - [Authentication handler](#authentication-handler)
  - [Schema](#schema)

## Authentication service

 In ASP.NET Core, authentication is handled by the `IAuthenticationService`, which is used by authentication middleware. The authentication service uses registered *authentication handlers* to complete authentication-related actions, like:

- Authenticating a user
- Responding when an unauthenticated user tries to access a restricted resource

## Authentication handler

An authentication handler:

- Is a type that implements the behavior of a *scheme*.
- Is derived from `IAuthenticationHandler` or `AuthenticationHandler<TOptions>`.

Based on the authentication scheme's configuration and the incoming request context, authentication handlers:

- Construct `AuthenticationTicket` objects representing the user's identity if authentication is successful.
- Return 'no result' or 'failure' if authentication is unsuccessful.
- Have methods for challenge and forbid actions for when users attempt to access resources:
  - They are unauthorized to access: `ForbidAsync` method.
  - When they are unauthenticated: `ChallengeAsync` method.

## Schema

A **schema** is a registered authentication handler and its *configuration options*.

Authentication schemes are specified by registering authentication services in `Startup.ConfigureServices`:

- By calling a scheme-specific extension method after a call to `services.AddAuthentication` (such as `AddJwtBearer` or `AddCookie`, for example). These extension methods use `AuthenticationBuilder.AddScheme` to register schemes with appropriate settings.
- Less commonly, by calling `AuthenticationBuilder.AddScheme` directly.

For example, the following code registers authentication services and handlers for cookie and JWT bearer authentication schemes:

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(JwtBearerDefaults.AuthenticationScheme, options => Configuration.Bind("JwtSettings", options))
    .AddCookie(CookieAuthenticationDefaults.AuthenticationScheme, options => Configuration.Bind("CookieSettings", options));
```

The `AddAuthentication` parameter `JwtBearerDefaults.AuthenticationScheme` is the name of the scheme to use by default when a specific scheme isn't requested.

There is no automatic probing of schemes. If the default scheme is not specified, the scheme must be specified in the authorize attribute, otherwise, the following error is thrown:

```text
InvalidOperationException: No authenticationScheme was specified, and there was no DefaultAuthenticateScheme found. The default schemes can be set using either AddAuthentication(string defaultScheme) or AddAuthentication(Action<AuthenticationOptions> configureOptions).
```
