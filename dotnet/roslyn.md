# Code analysis

[↑ Overview of .NET source code analysis](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview)

*Roslyn* analyzers inspect your C# or Visual Basic code for *code quality* and *style issues*.

[↑ Roslyn](https://github.com/dotnet/roslyn) is a .NET compiler platform. Roslyn includes implementation of both the C# and Visual Basic compilers with an API surface for building *code analysis* tools.

[↑ Code quality analysis rules](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/quality-rules), denoted as `CAxxxx`, inspect your C# code for security, performance, design and other issues.

[↑ Code-style analysis rules](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules), denoted as `IDExxxx`, enable you to define and maintain consistent code style in your codebase.

## Table of contents

- [Code analysis](#code-analysis)
  - [Table of contents](#table-of-contents)
  - [Code analyzers](#code-analyzers)
  - [Code analysis vs EditorConfig file](#code-analysis-vs-editorconfig-file)
  - [Severity levels of analyzers](#severity-levels-of-analyzers)
  - [Code analysis in CI builds](#code-analysis-in-ci-builds)
  - [Running analyzers](#running-analyzers)
  - [AnalysisMode](#analysismode)
  - [Analysis rule category](#analysis-rule-category)
  - [Language rules](#language-rules)

## Code analyzers

`Microsoft.CodeAnalysis.NetAnalyzers` is a primary analyzer package among [↑ Roslyn analyzers](https://github.com/dotnet/roslyn-analyzers). Included and enabled by default in .NET 5+ projects. You do not need to install `Microsoft.CodeAnalysis.NetAnalyzers` [↑ NuGet package](https://www.nuget.org/packages/Microsoft.CodeAnalysis.NetAnalyzers) to use it because it's a part of .NET SDK now.

If rule violations are found by an analyzer, they're reported as a suggestion, warning, or error, depending on how each rule is configured. Code analysis violations appear with the prefix `CA` or `IDE` to differentiate them from compiler errors.

Code quality analysis rules have `CAxxxx` format, for example `CA2017`, `CA2013`, etc.

## Code analysis vs EditorConfig file

Code analysis and [↑ EditorConfig](https://editorconfig.org) files work hand-in-hand. When you define code styles in an EditorConfig file you're actually configuring SDK's built-in code analyzers. EditorConfig files can be used to enable or disable analyzer rules, and also to configure NuGet analyzer packages.

## Severity levels of analyzers

[↑ Severity levels of analyzers](https://docs.microsoft.com/en-us/visualstudio/code-quality/roslyn-analyzers-overview#severity-levels-of-analyzers).

Each analyzer has one of the following severity levels: `error`, `warning`,  `suggestion`, `silent`, `none`, `default`.

## Code analysis in CI builds

For analyzers that are installed from a NuGet package, those rules are enforced at build time, including during a CI build. Currently, the code analyzers that are built into Visual Studio are not available as a NuGet package, and so these rules are not enforceable in a CI build.

## Running analyzers

.NET analyzers run on a build.

By default, only [↑ some rules](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview#enabled-rules) are enabled: see Code quality analysis: Enabled rules. To enable all rules, you may set `<AnalysisMode>AllEnabledByDefault</AnalysisMode>` in your `.csproj` or a `Directory.Build.props` file in the project directory or above.

## AnalysisMode

The `AnalysisMode` property lets you customize the set of rules that are enabled by default. You can either switch to a more aggressive analysis mode where you can opt out of rules individually, or a more conservative analysis mode where you can opt in to specific rules. For example, if you want to enable all rules as build warnings, set the value to `All`.

Available modes: `None`, `Default`, `Minimum`, `Recommended`, `All`.

## Analysis rule category

[↑ Rule categories](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/categories)

Each code analysis rule belongs to a category of rules. You can configure the severity level for an entire category of rules. You can also configure additional options on a per-category basis.

Available rule categories: `Design`, `Documentation`, `Globalization`, `Portability and interoperability`, `Maintainability`, `Naming`, `Performance`, `SingleFile`, `Reliability`, `Security`, `Style`, `Usage`.

## Language rules

[↑ Language rules](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/language-rules)

Code-style language rules affect how various constructs of .NET programming languages, for example, modifiers, and parentheses, are used. The rules fall into the following categories:

- .NET style rules: Rules that apply to both C# and Visual Basic. The option names for these rules start with the prefix `dotnet_style_`
- C# style rules: Rules that are specific to C# code. The option names for these rules start with the prefix `csharp_style_`
- Visual Basic style rules: Rules that are specific to Visual Basic code. The option names for these rules start with the prefix `visual_basic_style_`
