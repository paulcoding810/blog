---
date: "2025-07-11T22:20:20+07:00"
draft: false
title: "JS WTF?"
summary: "Exploring fascinating JavaScript."
categories:
  - Code
tags:
  - javascript
  - wtf
  - coding
---

JavaScript is fantastic, no argue. In this post we're diving into some fantastic syntaxes and features.

### Immediately Invoked Function Expression (IIFE)

They are functions that run as soon as it's defined. Ususally seen in bundled js files. It wont export any variable or method to the global scope.

```js
// Standard IIFE
(function () {
  console.log("Hello from IIFE!");
})();

// Arrow function IIFE
(() => {
  console.log("Arrow IIFE here!");
})();

// Async IIFE
(async () => {
  console.log("Async IIFE in action!");
})();
```

[Learn more about IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)

---

### The Comma Operator (`,`)

It lets you evaluate multiple expressions and returns the last one. Here's how it works:

```js
let x = 1;

x = (x++, x); // Increment x, then assign the new value of x
console.log(x); // Output: 2

x = (2, 3); // Only the last value matters
console.log(x); // Output: 3
```

[More on the comma operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator)

---

### Generator Functions (function\*)

Generators are like functions with a pause button. They let you yield values one at a time:

```js
function* generator(i) {
  yield i;
  yield i + 10;
}

const gen = generator(10);

console.log(gen.next().value); // Output: 10
console.log(gen.next().value); // Output: 20
```

[Deep dive into generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)

---

### Functions: A Quick Comparison

{{< gpt >}}

Here's a handy table comparing function declarations and expressions:

| Feature              | Function Declaration                  | Function Expression                   |
| -------------------- | ------------------------------------- | ------------------------------------- |
| **Hoisting**         | Yes (hoisted to the top of the scope) | No (only accessible after assignment) |
| **Syntax**           | `function name() { ... }`             | `const name = function() { ... }`     |
| **Usage**            | Can be called before it's defined     | Must be defined before calling        |
| **Anonymous?**       | No, always named                      | Yes, can be anonymous                 |
| **Arrow Functions?** | No                                    | Yes, supports arrow functions         |

---

### Quick CSS Styling with JavaScript

Neat trick to apply CSS:

```js
const style = {
  width: "50px",
  height: "50px",
  borderRadius: "25px", // CamelCase for CSS properties
};

Object.assign(container.style, style);
```

---

That's it for now! JavaScript is full of these little gems, so stay curious and keep exploring!
