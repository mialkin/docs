# Authorization

**Authorization** is the process of determining whether a user has access to a resource.

Authorization is orthogonal and independent from authentication. However, authorization requires an authentication mechanism.

ASP.NET Core authorization provides following authorization types:

- Simple
- Role-based
- Claims-based
- Policy-based
- Resource-based
- View-based

## Table of contents

- [Authorization](#authorization)
  - [Table of contents](#table-of-contents)
  - [Simple authorization](#simple-authorization)
  - [Role-based authorization](#role-based-authorization)
  - [Claims-based authorization](#claims-based-authorization)
  - [Policy-based authorization](#policy-based-authorization)
    - [Requirements](#requirements)

## Simple authorization

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

## Role-based authorization

When an identity is created it may belong to one or more *roles*. How these roles are created and managed depends on the backing store of the authorization process. Roles are exposed to the developer through the `IsInRole` method on the `ClaimsPrincipal` class.

When user is logged in, all user roles are added as claims with claims type being `ClaimTypes.Role` and values are role name.

And when you execute `IPrincipal.IsInRole("SuperAdmin")` the framework actually checks if the claim with type `ClaimTypes.Role` and value `SuperAdmin` is present on the user.

## Claims-based authorization

When an identity is created it may be assigned one or more *claims* issued by a trusted party. A claim is a name-value pair that represents what the subject is, not what the subject can do. Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.

An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.

Claims in code specify claims which the current user must possess, and optionally the value the claim must hold to access the requested resource. Claims *requirements* are policy based, the developer must build and register a policy expressing the claims requirements.

The simplest type of claim policy looks for the presence of a claim and doesn't check the value.

## Policy-based authorization

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

Apply policies to controllers by using the [Authorize] attribute with the policy name:

### Requirements

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

