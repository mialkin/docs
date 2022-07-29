# Middleware

A **middleware** is software that's assembled into an app pipeline to handle requests and responses.

The middleware class must include:

- A public constructor with a parameter of type `RequestDelegate`.
- A public method named `Invoke` or `InvokeAsync`. This method must:
  - Return a `Task`.
  - Accept a first parameter of type `HttpContext`.

Additional parameters for the constructor and `Invoke`/`InvokeAsync` are populated by dependency injection.

```csharp
using System.Globalization;

namespace Middleware.Example;

public class RequestCultureMiddleware
{
    private readonly RequestDelegate _next;

    public RequestCultureMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var cultureQuery = context.Request.Query["culture"];
        if (!string.IsNullOrWhiteSpace(cultureQuery))
        {
            var culture = new CultureInfo(cultureQuery);

            CultureInfo.CurrentCulture = culture;
            CultureInfo.CurrentUICulture = culture;
        }

        // Call the next delegate/middleware in the pipeline.
        await _next(context);
    }
}

public static class RequestCultureMiddlewareExtensions
{
    public static IApplicationBuilder UseRequestCulture(this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<RequestCultureMiddleware>();
    }
}
```

`Program.cs`:

```csharp
using Middleware.Example;
using System.Globalization;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.UseHttpsRedirection();

app.UseRequestCulture();

app.Run(async (context) =>
{
    await context.Response.WriteAsync(
        $"CurrentCulture.DisplayName: {CultureInfo.CurrentCulture.DisplayName}");
});

app.Run();
```

## Links

- [↑ ASP.NET Core Middleware](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/middleware)
- [↑ Write custom ASP.NET Core middleware](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/middleware/write)
