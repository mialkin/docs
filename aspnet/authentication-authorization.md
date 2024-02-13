# Authentication, Authorization

## Table of contents

- [Authentication, Authorization](#authentication-authorization)
  - [Table of contents](#table-of-contents)
  - [Authentication](#authentication)
    - [Authentication service](#authentication-service)
    - [Authentication handler](#authentication-handler)
    - [Schema](#schema)
    - [Links](#links)
  - [Authorization](#authorization)
    - [Authorization types](#authorization-types)
      - [Simple](#simple)
      - [Role-based](#role-based)
      - [Claims-based](#claims-based)
      - [Policy-based](#policy-based)
        - [Requirements](#requirements)
      - [Resource-based](#resource-based)
      - [View-based](#view-based)
    - [Links](#links-1)

## Authentication

An **authentication** is the process of determining user's identity.

Authentication may create *one or more identities* for the current user.

### Authentication service

 In ASP.NET Core, authentication is handled by the `IAuthenticationService`, which is used by authentication middleware. The authentication service uses registered *authentication handlers* to complete authentication-related actions, like:

- Authenticating a user
- Responding when an unauthenticated user tries to access a restricted resource

### Authentication handler

An authentication handler:

- Is a type that implements the behavior of a *scheme*.
- Is derived from `IAuthenticationHandler` or `AuthenticationHandler<TOptions>`.

Based on the authentication scheme's configuration and the incoming request context, authentication handlers:

- Construct `AuthenticationTicket` objects representing the user's identity if authentication is successful.
- Return 'no result' or 'failure' if authentication is unsuccessful.
- Have methods for challenge and forbid actions for when users attempt to access resources:
  - They are unauthorized to access: `ForbidAsync` method.
  - When they are unauthenticated: `ChallengeAsync` method.

### Schema

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

### Links

[↑ Overview of ASP.NET Core authentication](https://learn.microsoft.com/en-us/aspnet/core/security/authentication).

## Authorization

An **authorization** is the process of determining whether a user has access to a resource.

Authorization is orthogonal and independent from authentication. However, authorization requires an authentication mechanism.

### Authorization types

#### Simple

Authorization in ASP.NET Core is controlled with `AuthorizeAttribute` class and its various parameters. In its most basic form, applying the `[Authorize]` attribute to a controller or action limits access to that component to the authenticated users.

You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

[↑ Simple authorization in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/simple).

#### Role-based

When an identity is created it may belong to one or more *roles*. How these roles are created and managed depends on the backing store of the authorization process. Roles are exposed to the developer through the `IsInRole` method on the `ClaimsPrincipal` class.

When user is logged in, all user roles are added as claims with claims type being `ClaimTypes.Role` and values are role name.

And when you execute `IPrincipal.IsInRole("SuperAdmin")` the framework actually checks if the claim with type `ClaimTypes.Role` and value `SuperAdmin` is present on the user.

#### Claims-based

When an identity is created it may be assigned one or more *claims* issued by a trusted party. A claim is a name-value pair that represents what the subject is, not what the subject can do. Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.

An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.

Claims in code specify claims which the current user must possess, and optionally the value the claim must hold to access the requested resource. Claims *requirements* are policy based, the developer must build and register a policy expressing the claims requirements.

The simplest type of claim policy looks for the presence of a claim and doesn't check the value.

#### Policy-based

Underneath the covers, role-based authorization and claims-based authorization use:

- requirement
- authorization handler
- preconfigured policy

These building blocks support the expression of authorization evaluations in code. The result is a richer, reusable, testable authorization structure.

An authorization *policy* consists of one or more *requirements*.

Policy-base authorization is registered as part of the authorization service configuration, in the `Program.cs` file:

```csharp
builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("AtLeast21", policy =>
        policy.Requirements.Add(new MinimumAgeRequirement(21)));
});
```

Above an "AtLeast21" policy is created. It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.

Apply policies to controllers by using the `[Authorize]` attribute with the policy name:

##### Requirements

An **authorization requirement** is a collection of data parameters that a policy can use to evaluate the current user principal. In our "AtLeast21" policy, the requirement is a single parameter — the minimum age. A requirement implements `IAuthorizationRequirement`, which is an empty marker interface. A parameterized minimum age requirement could be implemented as follows:

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public MinimumAgeRequirement(int minimumAge) =>
        MinimumAge = minimumAge;

    public int MinimumAge { get; }
}
```

If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed. In other words, multiple authorization requirements added to a single authorization policy are treated on an *AND* basis.

> A requirement doesn't need to have data or properties.

#### Resource-based

[↑ Resource-based authorization in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/resourcebased).

#### View-based

[↑ View-based authorization in ASP.NET Core MVC](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/views).

### Links

[↑ Introduction to authorization in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/introduction).
