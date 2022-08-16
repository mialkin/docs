# Roslyn and Roslyn analyzers

## Roslyn

[↑ Roslyn](https://github.com/dotnet/roslyn) is a .NET compiler platform.

Roslyn includes implementation of both the C# and Visual Basic compilers with an API surface for building *code analysis tools*.

## Code analyzers

[↑ Roslyn analyzers](https://github.com/dotnet/roslyn-analyzers) inspect your C# or Visual Basic code for code quality and style issues.

`Microsoft.CodeAnalysis.NetAnalyzers` is a primary analyzer package among Roslyn analyzers. Included and enabled by default in .NET 5+ projects. You do not need to install `Microsoft.CodeAnalysis.NetAnalyzers` [↑ NuGet package](https://www.nuget.org/packages/Microsoft.CodeAnalysis.NetAnalyzers) to use it because it's a part of .NET SDK now.

Code quality analysis rules have `CAxxxx` format, for example `CA2017`, `CA2013`, etc.

[↑ FAQ for code analysis in Visual Studio](https://docs.microsoft.com/en-us/visualstudio/code-quality/analyzers-faq).

## Code analysis vs EditorConfig file

Code analysis and [↑ EditorConfig](https://editorconfig.org) files work hand-in-hand. When you define code styles in an EditorConfig file you're actually configuring SDK's built-in code analyzers. EditorConfig files can be used to enable or disable analyzer rules, and also to configure NuGet analyzer packages.

## Severity levels of analyzers

[↑ Severity levels of analyzers](https://docs.microsoft.com/en-us/visualstudio/code-quality/roslyn-analyzers-overview#severity-levels-of-analyzers)
