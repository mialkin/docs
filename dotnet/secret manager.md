# Secret Manager tool

In the project directory run:

```bash
dotnet user-secrets init
```

It will add a new `UserSecretsId` record to `.csproj` file:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <LangVersion>9</LangVersion>
        <UserSecretsId>8bf536a6-2591-43de-9052-0e6781dec11e</UserSecretsId>
    </PropertyGroup>
</Project>
```

From the project directory run:

```bash
dotnet user-secrets set "ConnectionStrings:SalesDatabase" "Server=localhost;Database=Sales;Trusted_Connection=True;"
```

List project's secrets:

```bash
dotnet user-secrets list
```

Project's secrets are stored in the file:

```bash
~/.microsoft/usersecrets/<user_secrets_id>/secrets.json
```

This `cat ~/.microsoft/usersecrets/8bf536a6-2591-43de-9052-0e6781dec11e/secrets.json` outputs:

```json
{
  "ConnectionStrings:SalesDatabase": "Server=localhost;Database=Sales;Trusted_Connection=True;"
}
```

## More

To see other available commands run:

```bash
dotnet user-secrets
```
