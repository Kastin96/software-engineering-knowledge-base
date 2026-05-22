# CommonJS vs ES Modules

## Goal

Understand the two common module systems used in Node.js.

## Why It Matters

Node.js projects may use CommonJS, ES modules, or a mix through dependencies.
Knowing the difference helps you read existing code and avoid import/export
errors.

## CommonJS

CommonJS uses `require` and `module.exports`.

```javascript
// math.cjs
function add(a, b) {
  return a + b;
}

module.exports = {
  add,
};
```

Import with `require`:

```javascript
// app.cjs
const { add } = require("./math.cjs");

console.log(add(2, 3));
```

CommonJS is older and still common in many Node.js projects.

## ES Modules

ES modules use `import` and `export`.

```javascript
// math.js
export function add(a, b) {
  return a + b;
}
```

Import with `import`:

```javascript
// app.js
import { add } from "./math.js";

console.log(add(2, 3));
```

For `.js` files, Node.js treats files as ES modules when `package.json` contains
`"type": "module"`.

```json
{
  "type": "module"
}
```

## File Extensions

CommonJS can use `.cjs`.

```javascript
// logger.cjs
module.exports = function log(message) {
  console.log(message);
};
```

ES modules can use `.mjs`.

```javascript
// logger.mjs
export function log(message) {
  console.log(message);
}
```

In a `"type": "module"` project, `.js` files are ES modules by default.

## Default and Named Exports

Named export:

```javascript
export function formatName(name) {
  return name.trim();
}
```

Default export:

```javascript
export default function createUser(name) {
  return {
    name,
  };
}
```

Import both:

```javascript
import createUser, { formatName } from "./users.js";
```

## Importing JSON

Depending on Node.js version and module type, JSON import syntax can differ.
When in doubt, reading JSON with `fs` is explicit and predictable.

```javascript
import { readFile } from "node:fs/promises";

const text = await readFile("data.json", "utf8");
const data = JSON.parse(text);

console.log(data);
```

## Real Pain Points

- Mixing `require` and `import` without understanding project module type often
  causes runtime errors.
- ES module imports usually need file extensions for local files in Node.js.
- `__dirname` and `__filename` exist in CommonJS, but not directly in ES
  modules.

```javascript
// ES module replacement for __dirname
import { fileURLToPath } from "node:url";
import path from "node:path";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

console.log(__dirname);
```

## Practice

1. Write one CommonJS module with `module.exports`.
2. Write one ES module with `export`.
3. Add `"type": "module"` to a sample `package.json`.
4. Explain why local ES module imports often include `.js`.

## Related Topics

- [Modules](../02_core_concepts/modules.md)
- [npm](npm.md)
- [File System, Path, and Process](fs_path_process.md)

