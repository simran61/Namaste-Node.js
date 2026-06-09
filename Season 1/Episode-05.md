# S01 E05 – Diving into the Node.js GitHub Repository

### Scope in Functions

- In JavaScript, all variables and functions defined inside a function are **private to that function’s scope**.

```js
function x() {
  const a = 10;

  function b() {
    console.log("b");
  }
}
console.log(a);
```

👇🏻 Output:

```txt
ReferenceError: a is not defined
```

- In the above example, variable `a` and function `b` can only be accessed inside function `x`.
- Whenever we wrap code inside a function, it creates a **private space** for itself.
- **Modules in Node.js work the same way.**

---

### Why variables & functions in modules are private?

- In Node.js, whenever we create a module, **all the code inside that module is wrapped inside a function** before execution.
- That’s why variables and functions inside a module cannot be accessed outside it.
- The only way to access them is through **`module.exports`**.

Example:
When you `require("./xyz.js")`, Node.js takes all the code from `xyz.js`, wraps it inside a function, and then executes it, preventing interference with other modules.

> **TL;DR:** You cannot access variables and functions of one module in another directly because they are wrapped inside a function.

---

### IIFE (Immediately Invoked Function Expression)

- When you use `require`, Node.js internally wraps your code in an **IIFE**.
- An IIFE is an **anonymous function invoked immediately**.

Why IIFE?

1. Executes the code immediately.
2. Provides **privacy** (keeps variables & functions safe).

```js
(function () {
  // all the code of the module runs here
})();
```

So when you do `require`, Node.js:

1. Wraps your module code in an IIFE.
2. Passes arguments like `require`, `module`, `exports`, etc.
3. Hands it over to the **V8 engine** for execution.

---

### Example – How Node.js wraps modules

```js
(function (module, require) {
  // all module code runs inside here
  // e.g. require('./sum.js')

  function calculateMultiply(a, b) {
    const result = a * b;
    console.log(result);
  }

  module.exports = { calculateMultiply };
})((module = {}));
```

✅ Node.js wraps code in an IIFE, passes parameters (`module`, `require`, etc.), and then executes it.
This keeps variables/functions **secure and private**.

---

![Node IIFE](image-69.png)

---

## The 5-Step Process of `require`

Whenever you use `require()`, this is what happens:

1. **Resolving the module**
   - Checks whether it’s:
     - A local file (`./path`)
     - A `.json` file
     - A Node internal module (`node:util`, etc.)

   - Resolves accordingly.

2. **Loading the module**
   - Loads file content based on type (JS/JSON/native).

3. **Compiling the module**
   - Wraps code inside an IIFE using Node’s internal `wrapper` function.

```js
let wrap = function (script) {
  // "script" is the full module code
  // 👇🏻 string concatenation
  return Module.wrapper[0] + script + Module.wrapper[1];
};

// wrapping in IFFE and passing exports, require etc
const wrapper = [
  "(function (exports, require, module, __filename, __dirname) { ",
  "\n});",
];
```

This results in:

```js
(function (exports, require, module, __filename, __dirname) {
  // all the code of your module
})();
```

4. **Code evaluation**
   - Executes the code and returns `module.exports`.

Example:

```js
const { calculateSum, calculateMultiply } = require("./calculate");
```

5. **Caching (important)**
   - A module’s code runs only **once**.
   - On subsequent `require()` calls, Node.js serves it from **cache** instead of re-executing.
   - Improves efficiency.

---

### Where is this in Node.js repo?

- **Node.js repo → `lib/` folder**: contains major JS code (e.g., `setTimeout`, utilities).
- Example:
  - `lib/timers.js` → contains `setTimeout` code → exported via `module.exports`.
  - `lib/internal/modules/esm/helpers.js` → `makeRequireFunction()` → creates the `require` function.
  - `lib/internal/modules/cjs/loader.js` → `Module.prototype.require`.

---

### Notes on `require`

```js
require("");
// ❌ TypeError [ERR_INVALID_ARG_VALUE]: The argument 'id' must be a non-empty string.
```

- `require` is implemented internally via `makeRequireFunction`.
- `Module.prototype.require` (inside `loader.js`) defines its behavior.

---

✅ That’s how Node.js makes `require()` work: wrapping, executing, exporting, and caching behind the scenes.

---
