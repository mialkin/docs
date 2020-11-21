# Middleware

Middleware is software that's assembled into an app pipeline to handle requests and responses. Each component:

* Chooses whether to pass the request to the next component in the pipeline.
* Can perform work before and after the next component in the pipeline.

Request delegates are used to build the request pipeline. The request delegates handle each HTTP request.

Request delegates are configured using [↑ Run](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.runextensions.run), [↑ Map](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.mapextensions.map), and [↑ Use](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.useextensions.use) extension methods:

* `Run` — adds a terminal middleware delegate to the application's request pipeline.

* `Map` — branches the request pipeline based on matches of the given request path. If the request path starts with the given path, the branch is executed.

* `Use` — adds a middleware delegate defined in-line to the application's request pipeline.

An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class. These reusable classes and in-line anonymous methods are **middleware**, also called **middleware components**. Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline. When a middleware short-circuits, it's called a **terminal middleware** because it prevents further middleware from processing the request.

The order that middleware components are added in the Startup.Configure method defines the order in which the middleware components are invoked on requests and the reverse order for the response. The order is **critical** for security, performance, and functionality.

## Links

* [↑ Middleware](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/middleware)
* [↑ Write custom ASP.NET Core middleware](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/middleware/write?view=aspnetcore-5.0)