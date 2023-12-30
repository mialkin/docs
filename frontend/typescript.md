# TypeScript

[↑ TypeScript](https://www.typescriptlang.org) is a free and open-source high-level programming language developed by Microsoft that adds static typing with optional type annotations to JavaScript.

TypeScript is designed for the development of large applications and transpiles to JavaScript.

A [↑ transpiler](https://en.wikipedia.org/wiki/Source-to-source_compiler) or **transcompiler** is a type of translator that takes the source code of a program written in a programming language as its input and produces an equivalent source code in the same or a different programming language.

## ECMAScript

[↑ ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) is a standard for scripting languages, including JavaScript, [↑ JScript](https://en.wikipedia.org/wiki/JScript), and [↑ ActionScript](https://en.wikipedia.org/wiki/ActionScript).

It is best known as a JavaScript standard intended to ensure the interoperability of web pages across different web browsers.

[↑ ECMAScript version history](https://en.wikipedia.org/wiki/ECMAScript_version_history).

## `tsconfig.json` file

The presence of a `tsconfig.json` file in a directory indicates that the directory is the root of a TypeScript project.

The `tsconfig.json` file specifies the root files and the compiler options required to compile the project.

### `include`

[↑ `include`](https://www.typescriptlang.org/tsconfig#include) specifies an array of filenames or patterns to include in the program. These filenames are resolved relative to the directory containing the `tsconfig.json` file.

### `exclude`

[↑ `exclude`](https://www.typescriptlang.org/tsconfig#exclude) specifies an array of filenames or patterns that should be skipped when resolving [`include`](#include).

### `compilerOptions`

[↑ `compilerOptions`](https://www.typescriptlang.org/tsconfig#compilerOptions) makes up the bulk of TypeScript's configuration and it covers how the language should work.
