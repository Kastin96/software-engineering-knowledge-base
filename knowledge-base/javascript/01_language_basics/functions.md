# Functions

## Goal

Understand how to create reusable blocks of logic.

## Why It Matters

Functions help you avoid repeating code. They make programs easier to read,
test, and change.

## Explanation

A function is a named block of code that can be called later.

```javascript
function sayHello() {
  console.log("Hello");
}

sayHello();
```

Functions can receive input through parameters.

```javascript
function greetUser(name) {
  console.log(`Hello, ${name}`);
}

greetUser("Alex");
greetUser("Sam");
```

Functions can return values.

```javascript
function add(a, b) {
  return a + b;
}

const result = add(2, 3);
console.log(result); // 5
```

Code after `return` inside the same function does not run.

```javascript
function getStatus(isActive) {
  if (!isActive) {
    return "inactive";
  }

  return "active";
}

console.log(getStatus(true)); // active
console.log(getStatus(false)); // inactive
```

## Function Declarations

Function declarations are common and readable.

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

console.log(calculateTotal(10, 3)); // 30
```

## Function Expressions

A function can also be stored in a variable.

```javascript
const calculateTotal = function (price, quantity) {
  return price * quantity;
};

console.log(calculateTotal(10, 3)); // 30
```

## Arrow Functions

Arrow functions are a shorter syntax often used in modern JavaScript.

```javascript
const calculateTotal = (price, quantity) => {
  return price * quantity;
};

console.log(calculateTotal(10, 3)); // 30
```

For a simple one-line return, you can write:

```javascript
const double = (number) => number * 2;

console.log(double(5)); // 10
```

## Default Parameters

Default parameters provide fallback values.

```javascript
function createGreeting(name = "Guest") {
  return `Hello, ${name}`;
}

console.log(createGreeting("Alex")); // Hello, Alex
console.log(createGreeting()); // Hello, Guest
```

## Functions With Objects

Functions often receive objects in real projects.

```javascript
function formatUser(user) {
  return `${user.name} <${user.email}>`;
}

const user = {
  name: "Alex",
  email: "alex@example.com",
};

console.log(formatUser(user));
```

## Common Mistakes

- Forgetting to call the function:

```javascript
function sayHello() {
  return "Hello";
}

console.log(sayHello); // function itself
console.log(sayHello()); // function result
```

- Forgetting `return`:

```javascript
function add(a, b) {
  a + b;
}

console.log(add(2, 3)); // undefined
```

Correct version:

```javascript
function add(a, b) {
  return a + b;
}
```

- Doing too much in one function:

```javascript
function isAdult(age) {
  return age >= 18;
}

function getAccessMessage(age) {
  if (isAdult(age)) {
    return "Access granted";
  }

  return "Access denied";
}
```

Small functions are easier to understand and reuse.

## Practice

1. Write a function that returns the square of a number.
2. Write a function that accepts a first name and last name and returns a full name.
3. Write a function that checks whether a user is active.
4. Rewrite one function as an arrow function.

## Related Topics

- [Variables](variables.md)
- [Data Types](data_types.md)
- [Control Flow](control_flow.md)
