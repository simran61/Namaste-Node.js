# S01 E04: Module, Export & Require

In a Node.js application, there is always an **entry point** - usually `app.js`, which you execute using:

```bash
node app.js
```

But what if you want to execute code from other files while keeping `app.js` as the entry point?
That’s where **modules** and the `require()` function come into play.

---

## Understanding Modules

Each file in Node.js is treated as a **module**.
For example, `xyz.js` and `app.js` are two separate modules.

To make them work together, we use the `require()` function.

```js
// xyz.js
console.log("this is xyz file");
```

```js
// app.js
require("./xyz.js"); // including one module into another
var name = "Simran Gangwani";
var a = 10;
var b = 20;
console.log(globalThis === this);
```

**Output (executed synchronously):**

```txt
this is xyz file
true
```

---

## Why `require()` Alone Isn’t Enough

```js
// sum.js
console.log("sum module is executed");

var x = "hello world";

function calculateSum(a, b) {
  const sum = a + b;
  console.log(sum);
}
```

```js
// app.js
require("./xyz.js");
require("./sum.js");

var a = 10;
var b = 20;

calculateSum(a, b); // ❌ ReferenceError: calculateSum is not defined
```

**Output:**

```txt
this is xyz file
sum module is executed
ReferenceError: calculateSum is not defined
```

### Reason for the Error

By default:

- Variables and functions inside a module are **private** to that module.
- Simply requiring a file executes it, but does **not** make its variables or functions accessible outside.

To share them, we must explicitly **export** them using the `module.exports` object.

---

## Exporting & Importing in Node.js

```js
// sum.js
console.log("sum module is executed");

function calculateSum(a, b) {
  const sum = a + b;
  console.log(sum);
}

// exporting the function
module.exports = calculateSum;
```

```js
// app.js
require("./xyz.js");

// importing the function
const calculateSum = require("./sum.js");

var a = 10;
var b = 20;

calculateSum(a, b);

console.log(globalThis === this);
```

**Output:**

```txt
this is xyz file
sum module is executed
30
true
```

---

## Exporting Multiple Values

To export both variables and functions, we wrap them inside an object:

```js
// sum.js
console.log("sum module is executed");

var x = "hello world";

function calculateSum(a, b) {
  console.log(a + b);
}

module.exports = { x, calculateSum }; // shorthand
```

### Importing the Object

```js
// app.js
const obj = require("./sum.js");

obj.calculateSum(10, 20);
console.log(obj.x);
```

Or via **destructuring**:

```js
const { x, calculateSum } = require("./sum.js");

calculateSum(10, 20);
console.log(x);
```

**Output:**

```txt
sum module is executed
30
hello world
```

---

## Why Protected Modules Are Useful

Protected variables/functions help avoid **naming conflicts** between different modules.

Example:
If `app.js` defines its own variable `x = 100` but only imports `calculateSum` from `sum.js`, the local `x` remains unaffected.

---

## CommonJS (CJS) vs ES Modules (ESM)

| Feature        | CommonJS (CJS)                | ES Modules (ESM)                                            |
| -------------- | ----------------------------- | ----------------------------------------------------------- |
| Syntax         | `module.exports`, `require()` | `export`, `import`                                          |
| Usage          | By default used in Node.js    | By default used in modern JavaScript (React, Angular, etc.) |
| Generation     | Older module system           | Newer ES6 standard                                          |
| Loading        | Synchronous                   | Supports asynchronous loading                               |
| Mode           | Non-strict mode by default    | Strict mode by default                                      |
| File Extension | `.js`, `.cjs`                 | `.mjs` or `.js` with `"type": "module"`                     |

By default, Node.js uses **CommonJS (CJS)** modules.

```json
// package.json
{
  "type": "commonjs"
}
```

For **ES Modules (ESM)**, specify:

```json
{
  "type": "module"
}
```

### Example: ES Module Export/Import

```js
// sum.js
export var x = "hello world";
export function calculateSum(a, b) {
  console.log(a + b);
}
```

```js
// app.js
import { x, calculateSum } from "./sum.js";

calculateSum(10, 20);
console.log(x);
```

---

## `module.exports` Explained

When a module is loaded, `module.exports` starts as an **empty object**:

```js
console.log(module.exports); // {}
```

We can assign values in two ways:

```js
// both are equivalent
module.exports = { x, calculateSum };
module.exports.x = x;
module.exports.calculateSum = calculateSum;
```

---

## Nested Modules

Consider a `calculate` folder with `sum.js` and `multiply.js`.

```js
// calculate/multiply.js
function calculateMultiply(a, b) {
  console.log(a * b);
}
module.exports = { calculateMultiply };
```

```js
// calculate/index.js
const { calculateMultiply } = require("./multiply");
const { calculateSum } = require("./sum");

module.exports = { calculateMultiply, calculateSum };
```

```js
// app.js
const { calculateSum, calculateMultiply } = require("./calculate");

calculateSum(10, 20);
calculateMultiply(10, 20);
```

**Output:**

```txt
30
200
```

✅ Here, the **entire `calculate` folder becomes a module**, and `app.js` doesn’t need to know its internal structure.

---

## Importing JSON Files

```js
// data.json
{
  "name": "Simran",
  "city": "Indore",
  "country": "India"
}
```

```js
// app.js
const data = require("./data.json");
console.log(data);
```

**Output:**

```txt
{ name: "Simran", city: "Indore", country: "India" }
```

---

## Built-in Node Modules

Node.js provides core modules, which can be imported directly:

```js
const util = require("node:util");
```

---

## What is a Module?

- A **module** can be a single file or a folder.
- It is a collection of JavaScript code that is **private by default**.
- Modules help organize, reuse, and protect code.

---
