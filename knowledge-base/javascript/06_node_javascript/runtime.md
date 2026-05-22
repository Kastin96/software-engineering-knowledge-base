# Runtime

## Goal

Understand what Node.js is and how JavaScript runs outside the browser.

## Why It Matters

Node.js is used for backend services, command-line tools, build scripts, test
runners, automation, package tooling, and many development workflows.

## Explanation

Node.js is a JavaScript runtime. It lets JavaScript run on a computer or server
instead of only inside a browser.

Browser JavaScript can work with the DOM. Node.js does not have the DOM by
default, but it provides APIs for files, paths, processes, networking, and
streams.

```javascript
console.log("Hello from Node.js");
```

Run the file:

```bash
node app.js
```

## Browser vs Node.js

Browser JavaScript:

```javascript
document.querySelector("#title").textContent = "Hello";
```

Node.js JavaScript:

```javascript
import { readFile } from "node:fs/promises";

const text = await readFile("message.txt", "utf8");

console.log(text);
```

The language is JavaScript in both places, but the available APIs are different.

## Built-In Modules

Node.js includes built-in modules.

```javascript
import path from "node:path";

const filePath = path.join("data", "users.json");

console.log(filePath);
```

The `node:` prefix makes it clear that the module is built into Node.js.

## Running a Script

Create `hello.js`:

```javascript
const name = "Alex";

console.log(`Hello, ${name}`);
```

Run it:

```bash
node hello.js
```

## Node Version

Check your Node.js version:

```bash
node --version
```

Different projects may require different Node versions. In real projects, this
is often documented in `README.md`, `.nvmrc`, `.node-version`, or `package.json`.

## Real Pain Points

- Browser-only APIs like `document`, `window`, and `localStorage` are not
  available in normal Node.js scripts.
- Different Node versions can support different JavaScript and runtime features.
- Long-running CPU-heavy code can still block the event loop in Node.js.

```javascript
// This works in the browser, but not in a normal Node.js script.
// console.log(document.title);
```

## Practice

1. Create a `hello.js` file and run it with `node`.
2. Print your Node.js version.
3. Import one built-in Node.js module.
4. Explain one difference between browser JavaScript and Node.js JavaScript.

## Related Topics

- [File System, Path, and Process](fs_path_process.md)
- [npm](npm.md)
- [Async JavaScript](../04_async_javascript/README.md)

