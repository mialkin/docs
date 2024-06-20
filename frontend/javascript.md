# ECMAScript, JavaScript, TypeScript

## Table of contents

- [ECMAScript, JavaScript, TypeScript](#ecmascript-javascript-typescript)
  - [Table of contents](#table-of-contents)
  - [ECMAScript](#ecmascript)
  - [JavaScript](#javascript)
    - [`let` vs `var` keywords](#let-vs-var-keywords)
    - [Spread syntax](#spread-syntax)
  - [TypeScript](#typescript)
    - [`tsconfig.json` file](#tsconfigjson-file)
      - [`include`](#include)
      - [`exclude`](#exclude)
      - [`compilerOptions`](#compileroptions)
        - [`target`](#target)
    - [`.ts` and `.tsx` extensions](#ts-and-tsx-extensions)

## ECMAScript

[↑ ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) is a standard for scripting languages, including [JavaScript](#javascript), [↑ JScript](https://en.wikipedia.org/wiki/JScript), and [↑ ActionScript](https://en.wikipedia.org/wiki/ActionScript).

It is best known as a JavaScript standard intended to ensure the interoperability of web pages across different web browsers.

[↑ ECMAScript version history](https://en.wikipedia.org/wiki/ECMAScript_version_history).

## JavaScript

**JavaScript** is a high-level language that conforms to the ECMAScript standard.

JavaScript has dynamic typing, prototype-based object-orientation, and first-class functions. It is multi-paradigm, supporting event-driven, functional, and imperative programming styles. It has APIs for working with text, dates, regular expressions, standard data structures, and the DOM.

### `let` vs `var` keywords

Variables declared by `let` have their scope in the block for which they are declared, as well as in any contained sub-blocks. In this way, `let` works very much like `var`. The main difference is that the scope of a `var` variable is the entire enclosing function:

```js
function varTest() {
    var x = 1;
    {
        var x = 2; // The same variable!
        console.log(x); // 2
    }
    console.log(x); // 2
}

function letTest() {
    let x = 1;
    {
        let x = 2; // Different variable
        console.log(x); // 2
    }
    console.log(x); // 1
}
```

At the top level of programs and functions `let`, unlike `var`, does not create a property on the global object:

```js
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```

### Spread syntax

Spread syntax `...` allows an iterable, such as an array expression or string, to be expanded in places where zero or more arguments, for function calls, or elements, for array literals, are expected, or an object expression to be expanded in places where zero or more key-value pairs, for object literals, are expected.

```js
function sum(x, y, z) {
    return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers)); // 6

```

```js
var parts = ["two", "three"];
var numbers = ["one", ...parts, "four", "five"]; // ["one", "two", "three", "four", "five"]
```

```js
// Just assume we have an object like this:
var person= {
    name: 'Alex',
    age: 35
}

// This:
<Modal {...person} title='Modal heading' animation={false} />

// is equal to:
<Modal name={person.name} age={person.age} title='Modal heading' animation={false} />
```

## TypeScript

[↑ TypeScript](https://www.typescriptlang.org) is a free and open-source high-level programming language developed by Microsoft that adds static typing with optional type annotations to [JavaScript](#javascript).

TypeScript is designed for the development of large applications and transpiles to JavaScript.

A [↑ transpiler](https://en.wikipedia.org/wiki/Source-to-source_compiler) or **transcompiler** is a type of translator that takes the source code of a program written in a programming language as its input and produces an equivalent source code in the same or a different programming language.

### `tsconfig.json` file

The presence of a `tsconfig.json` file in a directory indicates that the directory is the root of a TypeScript project.

The `tsconfig.json` file specifies the root files and the compiler options required to compile the project.

#### `include`

The [↑ `include`](https://www.typescriptlang.org/tsconfig#include) setting specifies an array of filenames or patterns to include in the program. These filenames are resolved relative to the directory containing the `tsconfig.json` file.

#### `exclude`

The [↑ `exclude`](https://www.typescriptlang.org/tsconfig#exclude) setting specifies an array of filenames or patterns that should be skipped when resolving [`include`](#include).

#### `compilerOptions`

The [↑ `compilerOptions`](https://www.typescriptlang.org/tsconfig#compilerOptions) setting makes up the bulk of TypeScript's configuration and it covers how the language should work.

##### `target`

The [↑ `target`](https://www.typescriptlang.org/tsconfig#target) setting changes which JS features are downleveled and which are left intact. For example, an arrow function `() => this` will be turned into an equivalent `function` expression if `target` is `ES5` or lower.

You might choose to set a lower target if your code is deployed to older environments, or a higher target if your code is guaranteed to run in newer environments.

Modern browsers support all ES6 features, so `ES6` is a good choice.

### `.ts` and `.tsx` extensions

The `.ts` extension is used fo pure TypeScript files.

The `.tsx` extension is used for TypeScript files that contain *JSX*.

[↑ JSX](https://react.dev/learn/writing-markup-with-jsx) is a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file. Although there are other ways to write components, most React developers prefer the conciseness of JSX, and most codebases use it.
