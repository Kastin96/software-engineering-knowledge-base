# Modules

## Goal

Understand how to split JavaScript code into files and share values between
them.

## Why It Matters

Real projects are too large for one file. Modules help organize code, reuse
logic, and keep implementation details private.

## Explanation

A module is a JavaScript file that can export values and import values from
other files.

Modern JavaScript uses ES modules with `export` and `import`.

## Named Exports

Use named exports when a file exports multiple values.

```javascript
// math.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

Import named exports with curly braces.

```javascript
// app.js
import { add, subtract } from "./math.js";

console.log(add(2, 3)); // 5
console.log(subtract(5, 2)); // 3
```

## Default Exports

Use a default export when the file has one main value.

```javascript
// User.js
export default class User {
  constructor(name) {
    this.name = name;
  }
}
```

Import a default export without curly braces.

```javascript
// app.js
import User from "./User.js";

const user = new User("Alex");

console.log(user.name);
```

## Mixing Default and Named Exports

You can use both, but keep it simple.

```javascript
// userService.js
export const defaultRole = "viewer";

export default function createUser(name) {
  return {
    name,
    role: defaultRole,
  };
}
```

```javascript
// app.js
import createUser, { defaultRole } from "./userService.js";

console.log(defaultRole);
console.log(createUser("Alex"));
```

## Module Scope

Values inside a module are private unless exported.

```javascript
// config.js
const internalApiUrl = "https://example.com/api";

export function getApiUrl() {
  return internalApiUrl;
}
```

Another file cannot import `internalApiUrl` unless it is exported.

## Import Aliases

You can rename imports.

```javascript
import { add as addNumbers } from "./math.js";

console.log(addNumbers(1, 2));
```

## Common Mistakes

- Forgetting the file extension in environments that require it:

```javascript
import { add } from "./math.js";
```

- Mixing named and default import syntax:

```javascript
// Named export
export function add(a, b) {
  return a + b;
}

// Correct named import
// import { add } from "./math.js";
```

- Creating circular dependencies:

```javascript
// Avoid designs where file A imports file B and file B imports file A.
```

Circular dependencies can make code harder to understand and test.

## Practice

1. Create a `math.js` module with `add` and `multiply`.
2. Import those functions in `app.js`.
3. Create a default export for a `User` class.
4. Create one private module variable and expose it through a function.

## Related Topics

- [Scope](scope.md)
- [Classes](classes.md)
- [Closures](closures.md)

