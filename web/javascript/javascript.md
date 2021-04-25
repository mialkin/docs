# JavaScript (JS)

**JavaScript** (**JS**) is a lightweight, interpreted, or just-in-time compiled programming language with first-class functions. While it is most well-known as the scripting language for Web pages, many non-browser environments also use it, such as Node.js, Apache CouchDB and Adobe Acrobat. JavaScript is a prototype-based, multi-paradigm, single-threaded, dynamic language, supporting object-oriented, imperative, and declarative (e.g. functional programming) styles.

## Table of content

* [Standard built-in objects](biltnobj/built-in%20objects.md)
  * [Promise](biltnobj/promise.md)

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
var parts = ['two', 'three'];
var numbers = ['one', ...parts, 'four', 'five']; // ["one", "two", "three", "four", "five"]
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

## Links

[↑ JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
