# Variables

## Goal

Understand how to store values using `const`, `let`, and `var`.

## Why It Matters

Variables allow a program to remember values and reuse them later. Almost every
JavaScript program uses variables.

## Explanation

A variable is a named place for a value.

```javascript
const username = "Alex";
console.log(username);
```

Use `const` when the variable should not be reassigned.

```javascript
const appName = "Task Manager";
console.log(appName);
```

Use `let` when the variable needs to change.

```javascript
let count = 0;

count = count + 1;
count = count + 1;

console.log(count); // 2
```

Avoid `var` in modern JavaScript. It is older and has function scope, which can
make code harder to understand.

```javascript
var oldStyle = "This still works, but prefer const or let";
console.log(oldStyle);
```

Variable names should describe the value clearly.

```javascript
const totalPrice = 120;
const isUserActive = true;
const userEmail = "alex@example.com";
```

## Reassignment vs Mutation

`const` prevents reassignment, but it does not make objects or arrays fully
unchangeable.

```javascript
const user = {
  name: "Alex",
  active: true,
};

user.active = false;

console.log(user); // { name: "Alex", active: false }
```

This is not allowed because it tries to assign a new value to the variable:

```javascript
const user = { name: "Alex" };

// Incorrect
// user = { name: "Sam" };
```

## Examples

Using `const`:

```javascript
const taxRate = 0.2;
const price = 100;
const tax = price * taxRate;

console.log(tax); // 20
```

Using `let`:

```javascript
let score = 10;

score = score + 5;
score = score - 2;

console.log(score); // 13
```

## Common Mistakes

- Using unclear names:

```javascript
const x = "alex@example.com"; // unclear
const userEmail = "alex@example.com"; // clearer
```

- Reassigning a `const` variable:

```javascript
const status = "pending";

// Incorrect
// status = "done";
```

- Using `var` by habit:

```javascript
// Avoid in modern code
var name = "Alex";

// Prefer
const userName = "Alex";
```

## Practice

1. Create a `const` variable for your favorite programming language.
2. Create a `let` variable called `level` and increase it by 1.
3. Create an object with `const` and update one property.

## Related Topics

- [Data Types](data_types.md)
- [Operators](operators.md)

