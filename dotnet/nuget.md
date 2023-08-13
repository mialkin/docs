# NuGet

**NuGet** is a Microsoft-supported mechanism for sharing code, which defines how packages for .NET are created, hosted, and consumed, and provides the tools for each of those roles.

**NuGet** is the package manager for .NET.

[↑ An introduction to NuGet](https://learn.microsoft.com/en-us/nuget/what-is-nuget).

## Table of contents

- [NuGet](#nuget)
  - [Table of contents](#table-of-contents)
  - [NuGet package](#nuget-package)
    - [Build NuGet package](#build-nuget-package)
    - [Acquire API key](#acquire-api-key)
    - [Publish package](#publish-package)
    - [Delete package](#delete-package)

## NuGet package

A **NuGet package** is a single ZIP file with the `.nupkg` extension that contains compiled code, i.e. DLLs, other files related to that code, and a descriptive manifest that includes information like the package's version number.

[↑ Quickstart: Create and publish a NuGet package using Visual Studio (Windows only)](https://learn.microsoft.com/en-us/nuget/quickstart/create-and-publish-a-package-using-visual-studio?tabs=netcore-cli).

### Build NuGet package

Run:

```bash
dotnet pack "src/Your.Models/Your.Models.csproj" \
--configuration Release \
--output ./build
```

This will create `Your.Models.1.0.0.nupkg` file under the `build` folder.

[↑ dotnet pack](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-pack).

### Acquire API key

Before you publish your NuGet package to [↑ nuget.org](https://www.nuget.org), create an [↑ API key](https://www.nuget.org/account/apikeys).

### Publish package

Check NuGet CLI version:

```bash
dotnet nuget --version
```

From the folder containing `.nupkg` file run:

```bash
dotnet nuget push build/Your.Models.1.0.0.nupkg \
--api-key qz2jga8ed2dvn2akkabcuwcs9ygggg4exypy3ovsy6w6x6 \
--source https://api.nuget.org/v3/index.json
```

[↑ dotnet nuget push](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-push).

When your package successfully publishes, you receive a confirmation email. To see the package you just published, on nuget.org, select your user name at upper right, and then select [↑ Manage Packages](https://www.nuget.org/account/Packages).

It might take awhile for your package to be indexed and appear in search results where others can find it. During that time, your package appears under Unlisted Packages. Package validation and indexing may take up to an hour. Nuget.org scans all uploaded packages for viruses and rejects the packages if it finds any viruses. Nuget.org also scans all existing listed packages periodically.

If you've created a package that isn't useful, such as a sample package that was created with an empty class library, or you decide you don't want the package to be visible, you can unlist the package to hide it from search results.

To avoid your test package being live on nuget.org, you can push to the nuget.org test site at <https://int.nugettest.org>. Note that packages uploaded to int.nugettest.org might not be preserved.

### Delete package

nuget.org does not support permanent deletion of packages. Doing so would break every project depending on the availability of the package, especially with build workflows that involve package restore.

nuget.org does support unlisting a package, which can be done in the package management page on the web site. Unlisted packages don't appear on nuget.org or in the Visual Studio UI, and do not appear in search results. Unlisted packages, however, can still be downloaded and installed by using an exact version number, which supports package restore.