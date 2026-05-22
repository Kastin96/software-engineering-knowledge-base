# ES6+ Features

## Goal

Understand the modern JavaScript features that appear in most current projects.

## Why It Matters

Modern JavaScript code relies on features added after the early versions of the
language. These features make code shorter, clearer, and easier to organize.

## const and let

Use `const` by default and `let` when reassignment is needed.

```javascript
const appName = "Dashboard";

let retryCount = 0;
retryCount += 1;

console.log(appName, retryCount);
```

## Template Literals

Template literals use backticks and support interpolation.

```javascript
const name = "Alex";
const score = 92;

const message = `${name} scored ${score}`;

console.log(message);
```

They can also span multiple lines.

```javascript
const email = `
Hello Alex,

Your report is ready.
`;

console.log(email);
```

## Arrow Functions

Arrow functions are common for callbacks and small functions.

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map((number) => number * 2);

console.log(doubled);
```

For object methods that use `this`, normal method syntax is usually clearer.

```javascript
const counter = {
  value: 0,
  increment() {
    this.value += 1;
  },
};
```

## Destructuring

Extract values from objects and arrays.

```javascript
const user = {
  id: 1,
  name: "Alex",
};

const { id, name } = user;

console.log(id, name);
```

Array destructuring:

```javascript
const colors = ["red", "green", "blue"];
const [firstColor, secondColor] = colors;

console.log(firstColor, secondColor);
```

## Spread and Rest

Spread copies or expands values.

```javascript
const user = {
  name: "Alex",
  active: true,
};

const updatedUser = {
  ...user,
  active: false,
};

console.log(updatedUser);
```

Rest collects remaining values.

```javascript
function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(1, 2, 3));
```

## Default Parameters

```javascript
function createGreeting(name = "Guest") {
  return `Hello, ${name}`;
}

console.log(createGreeting());
console.log(createGreeting("Alex"));
```

## Classes and Modules

Classes organize object behavior.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hello, ${this.name}`;
  }
}
```

Modules organize code across files.

```javascript
export function add(a, b) {
  return a + b;
}
```

```javascript
import { add } from "./math.js";

console.log(add(2, 3));
```

## Real Pain Points

- Object and array spread create shallow copies. Nested objects can still share
  references.
- Arrow functions do not have their own `this`, which is helpful in callbacks
  but wrong for some object methods.
- Modern syntax support depends on the runtime, build tools, and target browsers
  or Node.js version.

## Practice

1. Rewrite string concatenation with a template literal.
2. Rewrite a callback as an arrow function.
3. Use destructuring to read values from an object.
4. Create an updated object with spread syntax.

## Related Topics

- [Destructuring, Spread, and Rest](../03_data_structures/destructuring_spread_rest.md)
- [`this`](../02_core_concepts/this.md)
- [Modules](../02_core_concepts/modules.md)

