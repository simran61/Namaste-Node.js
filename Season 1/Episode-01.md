# S01 E01 - Introduction to Node.js

- **Node.js - Run JavaScript Everywhere**
- Node.js is a JavaScript runtime built on Chrome's V8 engine.
- Node.js is **cross-platform** (i.e., it can be run on any OS such as Windows, Linux, Unix, and macOS) and **open-source**. It is maintained by the **OpenJS Foundation** (a community-driven foundation responsible for Node.js development).
- It executes JavaScript code outside the browser, using the V8 engine.
- JavaScript gained popularity due to its dominance in web browsers. It powers dynamic behavior on websites and has always been the core of frontend development. With the advent of Node.js, JavaScript could now run outside the browser as well, unlocking backend capabilities.
- Node.js follows an **event-driven architecture** and is capable of **asynchronous I/O** operations (also known as non-blocking I/O), which is one of the reasons for its massive popularity.

---

## History of Node.js

- The first version of Node.js was built and released in **2009** by **Ryan Dahl**, an independent developer who began the project out of curiosity.

> To run JavaScript, we need a JS engine. Wherever there's JavaScript, there’s an engine executing it.

- Initially, Ryan used **SpiderMonkey** (Firefox's JS engine) but within two days switched to **V8** (Chrome’s JS engine) for better performance and reliability.
- Node.js was built on and powered by the V8 engine.
- Around the same time, a company named **Joyent** was working on similar ideas. Impressed by Ryan’s work, they hired him to continue Node.js development under their support.
- Early internal projects at Joyent used Node.js, helping it gain initial traction.
- Ryan initially named the project **web.js** as it was meant to build web servers. However, after realizing its broader potential, he renamed it to **Node.js** to reflect its wider applicability.

---

### Why was Node.js needed?

> Before Node.js, servers were typically created using Apache - a blocking server. Ryan wanted to create a **non-blocking** HTTP server capable of handling multiple requests using fewer threads.

---

## Rise of NPM

- In **2010**, a developer at Joyent created **NPM** (Node Package Manager) - a central registry where developers could publish and reuse JavaScript packages (e.g., for date formatting, image handling, etc.).
- NPM became a crucial part of Node.js’ success, enabling a strong open-source ecosystem.
- Initially, Node.js supported only **macOS and Linux**. In **2011**, Joyent collaborated with **Microsoft** to bring support for **Windows**, increasing adoption.

---

## Leadership Shift & Fork

- In **2012**, Ryan stepped away from the project (though still at Joyent). Leadership was handed over to **Isaac Schlueter**, creator of NPM.
- As Chrome kept releasing updated versions of the V8 engine, Node.js development began to lag. In **2014**, a developer named **Fedor Indutny** created a fork of Node.js called **io.js** to accelerate progress.
- A major **controversy** arose: Joyent was perceived as slowing down Node.js’ release cycle. For a year, two parallel projects existed:
  - `node.js` (original, maintained by Joyent)
  - `io.js` (fork, maintained by Fedor and others)

---

## Merger & Foundation

- In **September 2015**, both projects - `node.js` and `io.js` were merged.
- A new foundation, the **Node.js Foundation**, was formed to take over governance.
- Eventually, **JS Foundation** and **Node.js Foundation** merged into one, the **OpenJS Foundation** which now maintains the development and roadmap of Node.js.

---

## Summary

- Node.js gave developers the power to run JavaScript beyond the browser.
- It enabled building fast, scalable, event-driven, and non-blocking applications.
- NPM and community contributions played a vital role in its success.
- Governance shifted from individual leadership to open-source community-backed foundations for better scalability and transparency.

---
