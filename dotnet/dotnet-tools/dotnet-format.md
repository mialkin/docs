# `dotnet format`

[↑ `dotnet format`](https://github.com/dotnet/format) is a code formatter that applies style preferences to a project or solution.

Preferences will be read from an `.editorconfig` file, if present, otherwise a default set of preferences will be used.

## Table of contents

- [`dotnet format`](#dotnet-format)
  - [Table of contents](#table-of-contents)
  - [Commands](#commands)
  - [`.editorconfig` examples](#editorconfig-examples)
  - [Code analyzers](#code-analyzers)
    - [Microsoft.VisualStudio.Threading.Analyzers](#microsoftvisualstudiothreadinganalyzers)
      - [Rule prefixes](#rule-prefixes)
  - [Other code formatters](#other-code-formatters)

## Commands

| Command                             | Description                                                                                  |
| ----------------------------------- | -------------------------------------------------------------------------------------------- |
| `dotnet format whitespace`          | Fixes whitespace                                                                             |
| `dotnet format style`               | Runs code style analyzers                                                                    |
| `dotnet format analyzers`           | Runs third-party analyzers                                                                   |
| `dotnet format --verify-no-changes` | Formats but does not save. Returns a non-zero exit code if any files would have been changed |

## `.editorconfig` examples

[↑ runtime](https://github.com/dotnet/runtime/blob/main/.editorconfig).

[↑ dotnet format](https://github.com/dotnet/format/blob/main/.editorconfig).

[↑ Roslyn](https://github.com/dotnet/roslyn/blob/main/.editorconfig).

[↑ MessagePack](https://github.com/MessagePack-CSharp/MessagePack-CSharp/blob/master/.editorconfig).

[↑ aspnetcore](https://github.com/dotnet/aspnetcore/blob/main/.editorconfig).

## Code analyzers

### Microsoft.VisualStudio.Threading.Analyzers

- [↑ Microsoft.VisualStudio.Threading.Analyzers](https://github.com/microsoft/vs-threading/blob/main/doc/analyzers/installation.md).

You may want to use this analyzer because of the problem with `Async` in interfaces: [↑ link 1](https://github.com/dotnet/roslyn/issues/40050), [↑ link 2](https://youtrack.jetbrains.com/issue/RSRP-486739).

#### Rule prefixes

| Analyzer                | Prefix |
| ----------------------- | ------ |
| StyleCop.Analyzers      | `SA`   |
| SonarAnalyzers.CSharp   | `S`    |
| NET.CodeAnalyzers       | `CA`   |
| Visual Studio.Analyzers | `IDE`  |

[↑ Severity levels](https://learn.microsoft.com/en-us/visualstudio/code-quality/use-roslyn-analyzers).

## Other code formatters

[↑ Prettier-style line breaking](https://github.com/dotnet/format/issues/246).

[↑ CSharpier](https://csharpier.com), [↑ Rider plugin](https://plugins.jetbrains.com/plugin/18243-csharpier).

[↑ Overview of .NET source code analysis](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview).

[↑ Support Roslyn properties in editorconfig](https://youtrack.jetbrains.com/issue/RIDER-51400/Support-roslyn-properties-in-editorconfig).
