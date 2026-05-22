# Hoisting

## Goal

Understand how JavaScript handles declarations before code runs.

## Why It Matters

Hoisting explains why some variables and functions can be used before their
declaration and why some code throws errors. It is a common source of confusion
for beginners.

## Explanation

Hoisting means JavaScript prepares declarations before executing the code.

This does not mean JavaScript physically moves your code. It means the runtime
knows about some declarations before it starts running statements.

## Function Declarations

Function declarations can be called before they appear in the file.

```javascript
sayHello();

function sayHello() {
  console.log("Hello");
}
```

This works because the function declaration is hoisted.

## `var` Hoisting

`var` declarations are hoisted and initialized with `undefined`.

```javascript
console.log(count); // undefined

var count = 10;

console.log(count); // 10
```

JavaScript treats it roughly like this:

```javascript
var count;

console.log(count);

count = 10;
```

This behavior is one reason modern JavaScript usually avoids `var`.

## `let` and `const` Hoisting

`let` and `const` declarations are also hoisted, but they are not available
before the declaration line.

```javascript
// Incorrect
// console.log(name);

const name = "Alex";
console.log(name);
```

The time before the declaration can be used is called the temporal dead zone.

## Function Expressions

Function expressions follow variable rules.

```javascript
// Incorrect
// greet();

const greet = function () {
  console.log("Hello");
};

greet();
```

Arrow functions behave the same way when stored in `const` or `let`.

```javascript
// Incorrect
// add(2, 3);

const add = (a, b) => a + b;

console.log(add(2, 3));
```

## Practical Advice

Declare values before using them. This makes code easier to read and avoids
surprising behavior.

```javascript
const taxRate = 0.2;

function calculateTax(price) {
  return price * taxRate;
}

console.log(calculateTax(100));
```

## Common Mistakes

- Thinking `var` starts with the assigned value:

```javascript
console.log(total); // undefined, not 100

var total = 100;
```

- Calling an arrow function before it is created:

```javascript
// Incorrect
// printMessage();

const printMessage = () => {
  console.log("Message");
};
```

- Relying on hoisting to organize code:

```javascript
function runApp() {
  console.log("App started");
}

runApp();
```

This style is clearer than calling functions before the reader has seen them.

## Practice

1. Write a function declaration and call it before the declaration.
2. Write a `var` example and explain why it prints `undefined`.
3. Write a `const` function expression and call it after the declaration.
4. Rewrite a confusing hoisting example into clearer code.

## Related Topics

- [Scope](scope.md)
- [Functions](../01_language_basics/functions.md)
- [Strict Mode](strict_mode.md)

