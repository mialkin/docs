# Authorization

**Authorization** is the process of determining whether a user has access to a resource.

Authorization is orthogonal and independent from authentication. However, authorization requires an authentication mechanism.

ASP.NET Core authorization provides following authorization types:

- Simple
- Role-based
- Claims-based
- Policy-based

When user is logged in, all user roles are added as claims with claims type being `ClaimTypes.Role` and values are role name.

And when you execute `IPrincipal.IsInRole("SuperAdmin")` the framework actually checks if the claim with type `ClaimTypes.Role` and value `SuperAdmin` is present on the user.
