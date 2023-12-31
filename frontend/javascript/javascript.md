# JavaScript

**JavaScript** is a high-level language that conforms to the ECMAScript standard.

JavaScript has dynamic typing, prototype-based object-orientation, and first-class functions. It is multi-paradigm, supporting event-driven, functional, and imperative programming styles. It has APIs for working with text, dates, regular expressions, standard data structures, and the DOM.

## Table of contents

- [JavaScript](#javascript)
  - [Table of contents](#table-of-contents)
  - [`let` vs `var` keywords](#let-vs-var-keywords)
  - [Spread syntax](#spread-syntax)

## `let` vs `var` keywords

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

## Spread syntax

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
