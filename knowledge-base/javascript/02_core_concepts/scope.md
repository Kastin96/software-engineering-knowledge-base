# Scope

## Goal

Understand where variables can be accessed in JavaScript.

## Why It Matters

Scope helps you know which variables are available at each part of your code.
It prevents naming conflicts and makes programs easier to reason about.

## Explanation

Scope is the area where a variable can be used.

JavaScript has several common kinds of scope:

- global scope;
- function scope;
- block scope;
- module scope.

## Global Scope

A variable in global scope can be used from many places in the same environment.

```javascript
const appName = "Learning App";

function printAppName() {
  console.log(appName);
}

printAppName(); // Learning App
```

Global variables should be used carefully. Too many global values make code
harder to maintain.

## Function Scope

Variables created inside a function are available only inside that function.

```javascript
function createMessage() {
  const message = "Hello from a function";
  console.log(message);
}

createMessage();

// Incorrect
// console.log(message);
```

The variable `message` exists only inside `createMessage`.

## Block Scope

`let` and `const` are block-scoped. A block is code inside curly braces.

```javascript
if (true) {
  const status = "active";
  console.log(status);
}

// Incorrect
// console.log(status);
```

This also applies to loops.

```javascript
for (let index = 0; index < 3; index += 1) {
  console.log(index);
}

// Incorrect
// console.log(index);
```

## Nested Scope

Inner scopes can access values from outer scopes.

```javascript
const userName = "Alex";

function greetUser() {
  const greeting = "Hello";

  console.log(`${greeting}, ${userName}`);
}

greetUser();
```

Outer scopes cannot access values from inner scopes.

```javascript
function calculateTotal() {
  const total = 100;
  return total;
}

console.log(calculateTotal()); // 100

// Incorrect
// console.log(total);
```

## Shadowing

Shadowing happens when an inner variable has the same name as an outer variable.

```javascript
const status = "global";

function printStatus() {
  const status = "local";
  console.log(status);
}

printStatus(); // local
console.log(status); // global
```

Shadowing is valid, but too much of it can make code confusing.

## Module Scope

In modern JavaScript modules, variables are scoped to the file unless exported.

```javascript
const internalName = "Only this file can use this";

export const publicName = "Other files can import this";
```

## Common Mistakes

- Expecting a function variable to exist outside the function:

```javascript
function getUser() {
  const user = "Alex";
  return user;
}

console.log(getUser());
```

- Using `var` and expecting block scope:

```javascript
if (true) {
  var oldValue = "I am function-scoped or global-scoped";
}

console.log(oldValue);
```

Prefer `let` and `const`.

```javascript
if (true) {
  const value = "I am block-scoped";
  console.log(value);
}
```

## Practice

1. Create a global variable and use it inside a function.
2. Create a variable inside a function and try to explain why it cannot be used outside.
3. Create an `if` block with a `const` variable inside it.
4. Write an example of variable shadowing.

## Related Topics

- [Hoisting](hoisting.md)
- [Closures](closures.md)
- [Modules](modules.md)

