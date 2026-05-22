# Syntax Cheatsheet

## Variables

```javascript
const name = "Alex"; // no reassignment
let count = 0; // reassignment allowed

count += 1;
```

Prefer `const`. Use `let` only when reassignment is needed. Avoid `var` in
modern code.

## Primitive Values

```javascript
const text = "hello"; // string
const price = 19.99; // number
const active = true; // boolean
const empty = null; // intentional empty value
let missing; // undefined
const large = 123n; // bigint
const key = Symbol("key"); // symbol
```

## Objects

```javascript
const user = {
  id: 1,
  name: "Alex",
  active: true,
};

console.log(user.name);
console.log(user["active"]);
```

## Arrays

```javascript
const users = ["Alex", "Sam"];

console.log(users[0]);
console.log(users.length);
```

## Template Literals

```javascript
const user = "Alex";
const message = `Hello, ${user}`;
```

## Conditions

```javascript
if (score >= 90) {
  grade = "A";
} else if (score >= 70) {
  grade = "B";
} else {
  grade = "C";
}
```

## Switch

```javascript
switch (role) {
  case "admin":
    access = "full";
    break;
  case "editor":
    access = "write";
    break;
  default:
    access = "read";
}
```

## Loops

```javascript
for (let index = 0; index < items.length; index += 1) {
  console.log(items[index]);
}

for (const item of items) {
  console.log(item);
}

while (count < 3) {
  count += 1;
}
```

## Functions

```javascript
function add(a, b) {
  return a + b;
}

const multiply = function (a, b) {
  return a * b;
};

const divide = (a, b) => a / b;
```

## Default Parameters

```javascript
function greet(name = "Guest") {
  return `Hello, ${name}`;
}
```

## Destructuring

```javascript
const { id, name } = user;
const { email = "unknown@example.com" } = user;
const [first, second] = items;
```

## Spread

```javascript
const nextUser = {
  ...user,
  active: false,
};

const nextItems = [...items, "new item"];
```

## Rest

```javascript
const { passwordHash, ...publicUser } = user;

function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}
```

## Optional Chaining

```javascript
const city = user.profile?.address?.city;
const firstItemName = items[0]?.name;
callback?.();
```

## Nullish Coalescing

```javascript
const pageSize = settings.pageSize ?? 20;
```

Use `??` when `0`, `false`, or `""` are valid values.

## Classes

```javascript
class User {
  #token;

  constructor(name, token) {
    this.name = name;
    this.#token = token;
  }

  greet() {
    return `Hello, ${this.name}`;
  }

  hasToken() {
    return Boolean(this.#token);
  }
}
```

## Modules

```javascript
export function add(a, b) {
  return a + b;
}

export default class User {}
```

```javascript
import User, { add } from "./module.js";
```

## Errors

```javascript
throw new Error("Something failed");

try {
  riskyOperation();
} catch (error) {
  console.error(error.message);
} finally {
  cleanup();
}
```

## Type Checks

```javascript
typeof value === "string";
typeof value === "number";
Array.isArray(value);
value === null;
Number.isNaN(value);
```

## Equality

```javascript
value === otherValue;
value !== otherValue;
```

Prefer strict equality.

