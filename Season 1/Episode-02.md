# S01 E02 - JavaScript on the Server

- A **server** is a remote computer or CPU that receives and processes requests from other computers (clients).
- A **cloud server** is simply a remote machine that stays on 24/7 and listens for incoming requests.
- When you type `google.com` in your browser, the following happens:
  - Your browser (the client) sends a request.
  - The domain name (`google.com`) is mapped to an IP address (e.g., 114.265.123.8).
  - That IP address points to a machine/server somewhere in the world.
- So, typing `google.com` means your computer is making a request to a remote server over the internet.

---

## Using JavaScript on the Server

- Traditionally, JavaScript was only used in browsers. With **Node.js**, JavaScript can now run on the server too.
- This made it possible for developers to use JavaScript for both frontend and backend, enabling the rise of **full-stack JavaScript**.

---

## Node.js and the V8 Engine

- **Node.js** is built using **C++** and **JavaScript**.
- At its core, it embeds **V8**, the JavaScript engine from Chrome.
- **V8** is a C++ program that executes JavaScript code. It is not a physical entity, but a piece of software code.
- So when we say a JS engine executes JavaScript, we mean it’s a C++ program that understands and runs JS.

- The V8 GitHub repository is composed of:
  - ~72.1% C++
  - ~25.6% JavaScript
- Running JavaScript using an engine like V8 means running C++ code under the hood.

---

## What is V8?

- **V8** is Google’s open-source, high-performance JavaScript and WebAssembly engine written in C++.
- It is used in Chrome, Node.js, and other environments.
- It implements the **ECMAScript** and **WebAssembly** standards.
- V8 can run on Windows, macOS, and Linux, and supports x64, IA-32, and ARM processors.
- V8 can also be embedded into any C++ application.

### How V8 Executes JavaScript

- You write JavaScript code.
- V8 parses the code into an Abstract Syntax Tree (AST).
- It compiles the code into **bytecode**, and then **machine code** using JIT (Just-In-Time) compilation.
- This machine code is what the computer ultimately understands and executes.

---

## Node.js = V8 + Superpowers

- Since V8 can be embedded into any C++ application, it was embedded into Node.js.
- Node.js is a C++ application that includes the V8 engine and provides extra capabilities on top of it.

### Why V8 Alone Isn’t Enough

- V8 only follows **ECMAScript standards**, which are meant for the browser.
- It does not provide access to server-related capabilities like:

  - File system access
  - Network operations (e.g., HTTP calls)
  - Database connections

- Node.js adds these capabilities through custom APIs and modules written in C++ and JavaScript.

> **V8 + Additional APIs = Node.js**

- According to the Node.js GitHub repository:
  - ~62% of the code is JavaScript
  - ~21% is C++
- This means Node.js is a C++ program enriched with many JavaScript APIs that developers can use.

---

## JavaScript Execution Flow

JavaScript (high-level)  
→ C++ (JS engine like V8)  
→ Machine Code (binary)  
→ Executed by computer hardware

- As developers, we write JavaScript code.
- The C++-based V8 engine compiles it into machine-level code that the computer can understand and run.

---

## Node.js Internals

- In the Node.js GitHub repository, there is a `deps` folder (short for dependencies).
- This folder includes V8 and other libraries like:
  - `libuv` for asynchronous I/O and the event loop
  - `http_parser`, `zlib`, etc.
- This shows that Node.js includes V8 as a **dependency**, not as something it built from scratch.

---

## ECMAScript Standards

- **ECMAScript** is the standard that defines how JavaScript should work.
- It is maintained by the **TC39 committee**, a group of JavaScript developers and browser vendors.
- These standards are published by **ECMA International** in a document called **ECMA-262**.

- All JavaScript engines must follow ECMAScript specifications:

  - **V8** – used by Google Chrome and Node.js
  - **SpiderMonkey** – used by Mozilla Firefox
  - **Chakra** – used by Internet Explorer and legacy Edge
  - **JavaScriptCore** – used by Safari

- While each browser implements its own JavaScript engine, they all:
  - Follow ECMAScript standards
  - Perform the same job: parse JS code and convert it into machine-executable code

---

## Summary

- V8 is a C++ engine that executes JavaScript by compiling it to machine code.
- Node.js is a runtime environment that embeds V8 and provides additional APIs required for server-side development.
- Node.js allows JavaScript to run outside the browser, enabling developers to write backend code using the same language as the frontend.
- ECMAScript provides a standardized set of rules for how JavaScript engines should behave.
- The TC39 committee maintains and updates these ECMAScript specifications.
