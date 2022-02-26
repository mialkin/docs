# Authentication

In ASP.NET Core, authentication is handled by the `IAuthenticationService` service, which is used by authentication middleware. The authentication service uses registered *authentication handlers* to complete authentication-related actions.

The registered authentication handlers and their *configuration options* are called **schemes**.

Authentication schemes are specified by registering authentication services in `Startup.ConfigureServices`:

- By calling a scheme-specific extension method after a call to `services.AddAuthentication` (such as `AddJwtBearer` or `AddCookie`, for example). These extension methods use `AuthenticationBuilder.AddScheme` to register schemes with appropriate settings.
- Less commonly, by calling `AuthenticationBuilder.AddScheme` directly.

The AddAuthentication parameter JwtBearerDefaults.AuthenticationScheme is the name of the scheme to use by default when a specific scheme isn't requested.
