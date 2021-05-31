# JavaScript

- [JavaScript](#javascript)
  - [`let` keyword](#let-keyword)
  - [Spread syntax](#spread-syntax)
  - [Standard built-in objects](#standard-built-in-objects)
  - [Links](#links)

## `let` keyword

Variables declared by `let` have their scope in the block for which they are declared, as well as in any contained sub-blocks. In this way, `let` works very much like `var`. The main difference is that the scope of a `var` variable is the entire enclosing function:

```js
function varTest() {
    var x = 1;
    {
        var x = 2; // same variable!
        console.log(x); // 2
    }
    console.log(x); // 2
}

function letTest() {
    let x = 1;
    {
        let x = 2; // different variable
        console.log(x); // 2
    }
    console.log(x); // 1
}
```

At the top level of programs and functions, `let`, unlike `var`, does not create a property on the global object:

```js
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```


## Spread syntax

**Spread syntax** (...) allows an iterable such as an array expression or string to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected.

```js
function sum(x, y, z) {
    return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// expected output: 6
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

## Standard built-in objects

[↑ Standard built-in objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)

## Links

[↑ JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
