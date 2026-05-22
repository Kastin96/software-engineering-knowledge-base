# Strict Mode

## Goal

Understand what strict mode is and why modern JavaScript code often behaves as
if strict mode is enabled.

## Why It Matters

Strict mode catches some mistakes earlier and removes several unsafe JavaScript
behaviors. Modules and classes use strict mode automatically.

## Explanation

Strict mode is a stricter version of JavaScript execution.

You can enable it in a script or function with `"use strict";`.

```javascript
"use strict";

const name = "Alex";

console.log(name);
```

In modern code, ES modules and classes are strict by default.

## Preventing Accidental Globals

Without strict mode, assigning to an undeclared variable can create a global
variable in some environments.

```javascript
"use strict";

// Incorrect
// username = "Alex";
```

Correct code declares the variable.

```javascript
"use strict";

const username = "Alex";

console.log(username);
```

## `this` Behavior

In strict mode, a regular function called without an object has `this` as
`undefined`.

```javascript
"use strict";

function showThis() {
  return this;
}

console.log(showThis()); // undefined
```

This helps reveal mistakes where a function accidentally depends on global
context.

## Duplicate Parameters

Strict mode does not allow duplicate parameter names.

```javascript
"use strict";

// Incorrect
// function sum(a, a) {
//   return a + a;
// }
```

Use clear unique names.

```javascript
"use strict";

function sum(a, b) {
  return a + b;
}

console.log(sum(2, 3));
```

## Modules Are Strict By Default

You do not need to write `"use strict";` inside ES modules.

```javascript
// user.js
export function createUser(name) {
  return {
    name,
  };
}
```

This module already runs in strict mode.

## Classes Are Strict By Default

Class bodies also use strict mode automatically.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  getName() {
    return this.name;
  }
}

const user = new User("Alex");

console.log(user.getName());
```

## Common Mistakes

- Forgetting to declare variables:

```javascript
"use strict";

const total = 100;

console.log(total);
```

- Thinking strict mode changes JavaScript into a different language:

Strict mode does not change the main syntax. It mainly removes unsafe behavior
and makes some errors easier to catch.

- Adding `"use strict";` to every module:

```javascript
// In ES modules, this is usually unnecessary.
export const status = "active";
```

## Practice

1. Write a small script with `"use strict";`.
2. Try to explain why undeclared variables are dangerous.
3. Create a module and explain why it is strict by default.
4. Create a class and explain why its methods are strict by default.

## Related Topics

- [Modules](modules.md)
- [Classes](classes.md)
- [`this`](this.md)
