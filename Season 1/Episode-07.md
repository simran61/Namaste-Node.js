# S01 E07 – Sync vs Async, `setTimeout(0)`, and Blocking Code

## Synchronous Code Example

```javascript
console.log("Hello World");

var a = 1078698;
var b = 28986;

function multiplyFn(x, y) {
  const result = a * b;
  return result;
}

var c = multiplyFn(a, b);

console.log("multiplication result is: ", c);
```

**Output**

```
Hello World
Multiplication result is: 22637556228
```

---

## Asynchronous Code Example

```javascript
const fs = require("fs");
const https = require("https");

console.log("hello world");

var a = 1078698;
var b = 20986;

// async function
https.get("https://dummyjson.com/products/1", (res) => {
  console.log("fetched data successfully");
});

// async function
setTimeout(() => {
  console.log("setTimeout called after 5 seconds");
}, 5000);

// async function
fs.readFile("./file.txt", "utf8", (err, data) => {
  console.log("file data: ", data);
});

function multiplyFn(x, y) {
  const result = a * b;
  return result;
}

var c = multiplyFn(a, b);

console.log("multiplication result is: ", c);
```

**file.txt**

```
This is the file data
```

**Console Output**

```
hello world
multiplication result is: 34567543452
file data: this is file data
fetched data successfully
setTimeout called after 5 seconds
```

- Notice how the main thread was not blocked.
- Heavy tasks were offloaded to **libuv**.

---

## Blocking the Main Thread

- `fs.readFile` → async (non-blocking)
- `fs.readFileSync` → sync (blocking)

Even though V8 offloads file operations to libuv, the synchronous variant will **block the thread until completion**.

**Note**: synchronous functions like `fs.readFileSync` do not accept callbacks.

```javascript
const fs = require("fs");
const https = require("https");

console.log("hello world");

var a = 1078698;
var b = 20986;

// synchronous (blocking)
fs.readFileSync("./file.txt", "utf8");
console.log("this will execute only after file read");

// async
https.get("https://dummyjson.com/products/1", (res) => {
  console.log("fetched data successfully");
});

setTimeout(() => {
  console.log("setTimeout called after 5 seconds");
}, 5000);

fs.readFile("./file.txt", "utf8", (err, data) => {
  console.log("file data: ", data);
});

function multiplyFn(x, y) {
  const result = a * b;
  return result;
}

var c = multiplyFn(a, b);

console.log("multiplication result is: ", c);
```

**Console Output**

```
hello world
(this line is blocked until file read completes)
this will execute only after file read
multiplication result is: 34567543452
file data: this is file data
fetched data successfully
setTimeout called after 5 seconds
```

---

## Core Node Modules

1. `console`
2. `http`
3. `https`
4. `REPL`
5. `zlib` → compression
6. `crypto` → cryptography
7. `fs` → file system
8. `dns`
9. `domain`
10. `errors`
11. `utilities`
12. `v8`

---

## Crypto Module

### Async Example

```javascript
const crypto = require("node:crypto");

console.log("Hello World");

var a = 1078698;
var b = 20986;

crypto.pbkdf2("password", "salt", 5000000, 50, "sha512", (err, key) => {
  console.log("key is generated");
});

function multiplyFn(x, y) {
  const result = a * b;
  return result;
}

var c = multiplyFn(a, b);

console.log("Multiplication result is: ", c);
```

**Output**

```
Hello World
Multiplication result is: 22637556228
Key is generated
```

### Blocking Example

```javascript
const crypto = require("node:crypto");

console.log("hello world");

var a = 6549322;
var b = 25428;

// sync (blocking) – avoid using
crypto.pbkdf2Sync("password", "salt", 50000000, 50, "sha512");
console.log("first key is generated");

// async – offloaded to libuv
crypto.pbkdf2("password@123", "salt", 500000, 50, "sha512", (err, key) => {
  console.log("second key is generated");
});

function multiplyFn(x, y) {
  const result = a * b;
  return result;
}

var c = multiplyFn(a, b);

console.log("Multiplication result is: ", c);
```

**Output**

```
hello world
(first key blocks the thread)
first key is generated
multiplication result is: 34567543452
(second key generated asynchronously)
second key is generated
```

- Key generation is CPU-intensive.
- `pbkdf2Sync` blocks the main thread.
- `pbkdf2` is async and offloaded to **libuv**.

**pbkdf2 parameters**

- `"password"` → raw password input
- `"salt"` → salt value for encryption
- `50000000` → iterations (higher = stronger, slower)
- `50` → key length
- `"sha512"` → algorithm/digest used

---

## setTimeout

### Normal Usage

```javascript
console.log("Hello World");

var a = 1078698;
var b = 2098;

setTimeout(() => {
  console.log("call me after 3 seconds");
}, 3000);

function multiplyFn(x, y) {
  const result = a * b;
  return result;
}

var c = multiplyFn(a, b);

console.log("Multiplication result is: ", c);
```

**Output**

```
Hello World
Multiplication result is: 4675423345
call me after 3 seconds
```

### setTimeout(0) – Trust Issues

```javascript
console.log("hello world");

var a = 324543354;
var b = 234587;

setTimeout(() => {
  console.log("call me right now");
}, 0);

setTimeout(() => {
  console.log("call me after 3 seconds");
}, 3000);

function multiplyFn(x, y) {
  const result = a * b;
  return result;
}

var c = multiplyFn(a, b);

console.log("multiplication result is: ", c);
```

**Output**

```
hello world
multiplication result is: 4675423345
call me right now
call me after 3 seconds
```

Explanation:

- The callback with `setTimeout(0)` is **not executed immediately**.
- It runs only after the call stack is **empty**.
- `0 ms` means _schedule as soon as possible after current execution finishes_.

---

## Another Blocking Example

```javascript
const crypto = require("node:crypto");

console.log("hello world");
console.log("====");

// sync – blocks thread
crypto.pbkdf2Sync("password", "salt", 50000000, 50, "sha512");
console.log("first key is generated");

// executes only when call stack is empty
setTimeout(() => {
  console.log("call me right now!!!!!");
}, 0);

// async
crypto.pbkdf2("password@123", "salt", 5000000, 50, "sha512", (err, key) => {
  console.log("second key is generated");
});

var a = 6549322;
var b = 25428;

function multiplyFn(x, y) {
  const result = a * b;
  return result;
}

var c = multiplyFn(a, b);

console.log("multiplication result is: ", c);
```

**Output**

```
hello world
====
(big pause due to sync pbkdf2)
first key is generated
multiplication result is: 3456786509789
call me right now!!!!!
(second key generated asynchronously)
second key is generated
```

---

## This episode highlights:

- Difference between sync and async functions
- Blocking vs non-blocking behavior
- Importance of offloading tasks to libuv
- Subtle behavior of `setTimeout(0)`

---
