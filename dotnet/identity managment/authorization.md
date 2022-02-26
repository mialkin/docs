# Authorization

When user is logged in, all user roles are added as claims with claims type being `ClaimTypes`.Role and values are role name.

And when you execute `IPrincipal.IsInRole("SuperAdmin")` the framework actually checks if the claim with type `ClaimTypes.Role` and value `SuperAdmin` is present on the user.
