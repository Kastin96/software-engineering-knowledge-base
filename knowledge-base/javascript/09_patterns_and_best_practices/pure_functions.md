# Pure Functions

## Goal

Understand functions that return the same output for the same input and avoid
side effects.

## Why It Matters

Pure functions are easier to test, reuse, debug, and combine. They are useful in
business logic, data transformations, validation, reducers, and calculations.

## Explanation

A pure function:

- uses its inputs;
- returns a value;
- does not change outside state;
- does not depend on changing outside state.

```javascript
function add(a, b) {
  return a + b;
}

console.log(add(2, 3)); // 5
console.log(add(2, 3)); // 5
```

## Pure Data Transformation

```javascript
function getActiveUsers(users) {
  return users.filter((user) => user.active);
}

const users = [
  { name: "Alex", active: true },
  { name: "Sam", active: false },
];

console.log(getActiveUsers(users));
```

## Pure Validation

```javascript
function validateEmail(email) {
  if (!email.includes("@")) {
    return "Email must be valid";
  }

  return null;
}

console.log(validateEmail("alex@example.com"));
console.log(validateEmail("bad-email"));
```

## Side Effects

A side effect is anything a function does besides returning a value.

Examples:

- logging;
- changing a global variable;
- modifying an argument;
- writing to a file;
- making an HTTP request;
- changing the DOM;
- reading the current time.

```javascript
let count = 0;

function incrementCount() {
  count += 1;
  return count;
}
```

This function is not pure because it changes outside state.

## Separate Pure Logic From Side Effects

```javascript
function buildUserMessage(user) {
  return `Welcome, ${user.name}`;
}

function showUserMessage(user) {
  const message = buildUserMessage(user);
  document.querySelector("#message").textContent = message;
}
```

The message-building logic is pure. The DOM update is a side effect.

## Pure Functions With Objects

```javascript
function deactivateUser(user) {
  return {
    ...user,
    active: false,
  };
}

const user = {
  name: "Alex",
  active: true,
};

console.log(deactivateUser(user));
console.log(user);
```

## Real Pain Points

- A function that reads global state can change behavior without changing its
  arguments.
- A function that mutates arguments makes callers harder to reason about.
- Pure functions do not remove side effects from the application. They move side
  effects to clear boundaries.
- Date/time and random values make functions harder to test unless passed in as
  inputs.

```javascript
function isExpired(expiresAt, now) {
  return expiresAt.getTime() <= now.getTime();
}
```

Passing `now` in makes the function easier to test.

## Practice

1. Write a pure function that calculates a total price.
2. Write a pure function that validates a user object.
3. Refactor a function so it returns a new object instead of mutating an argument.
4. Move a console log out of a calculation function.

## Related Topics

- [Immutability](immutability.md)
- [Functional Patterns](functional_patterns.md)
- [Testing](../10_testing/README.md)

