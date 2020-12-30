# Generate self-signed certificate

On macOS:

```bash
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p YOUR_PASSWORD

dotnet dev-certs https --trust
```

Create a docker-compose.debug.yml file with the following content:

```yml
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=YOUR_PASSWORD
      - ASPNETCORE_Kestrel__Certificates__Default__Path=C:\https\aspnetapp.pfx
    volumes:
      - ${USERPROFILE}\.aspnet\https:C:\https:ro
```

Start the container with ASP.NET Core configured for HTTPS:

```bash
docker-compose -f "docker-compose.debug.yml" up -d
```

## Links

* [↑ Generate self-signed certificates with the .NET CLI](https://docs.microsoft.com/en-us/dotnet/core/additional-tools/self-signed-certificates-guide)
