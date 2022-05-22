# FluentDocker

[↑ FluentDocker](https://github.com/mariotoffia/FluentDocker) is a .NET library that enables `docker` and `docker-compose` interactions using Fluent API.

## Installation

```bash
dotnet add package Ductus.FluentDocker
```


## Usage example

```csharp
using Ductus.FluentDocker.Builders;
using Ductus.FluentDocker.Extensions;
using Ductus.FluentDocker.Services;
using Ductus.FluentDocker.Services.Extensions;
using FluentAssertions;

var builder = new Builder();

var containerService = builder
    .UseContainer()
    .UseImage("postgres:12")
    .ExposePort(5432)
    .WithEnvironment("POSTGRES_PASSWORD=mysecretpassword")
    .WaitForPort("5432/tcp", millisTimeout: 30000 /*30s*/)
    .Build()
    .Start();

int hostPort = containerService.ToHostExposedEndpoint(portAndProto: $"{5432}/tcp").Port;

var container = containerService.GetConfiguration(fresh: true);

container.State.ToServiceState().Should().Be(ServiceRunningState.Running);

containerService.Stop();
containerService.Remove();
```
