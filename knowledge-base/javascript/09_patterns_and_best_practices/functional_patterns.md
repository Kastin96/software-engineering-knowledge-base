# Functional Patterns

## Goal

Use functions to transform data clearly and predictably.

## Why It Matters

Functional patterns are common in JavaScript because functions are values. They
help with data pipelines, array transformations, validation, event handling, and
configuration.

## Functions as Values

You can store a function in a variable.

```javascript
const double = (number) => number * 2;

console.log(double(5));
```

You can pass a function to another function.

```javascript
function applyToValue(value, transform) {
  return transform(value);
}

console.log(applyToValue(5, double));
```

## map, filter, and reduce

Use `map` to transform items.

```javascript
const users = [
  { name: "Alex" },
  { name: "Sam" },
];

const names = users.map((user) => user.name);

console.log(names);
```

Use `filter` to keep matching items.

```javascript
const activeUsers = users.filter((user) => user.active);
```

Use `reduce` when you need one result from many items.

```javascript
const prices = [10, 20, 30];

const total = prices.reduce((sum, price) => sum + price, 0);

console.log(total);
```

## Function Composition

Composition means combining small functions.

```javascript
function trim(value) {
  return value.trim();
}

function lowercase(value) {
  return value.toLowerCase();
}

function normalizeEmail(email) {
  return lowercase(trim(email));
}

console.log(normalizeEmail("  ALEX@EXAMPLE.COM  "));
```

## Higher-Order Functions

A higher-order function accepts a function or returns a function.

```javascript
function createMinLengthValidator(minLength) {
  return function validate(value) {
    return value.length >= minLength;
  };
}

const isLongEnough = createMinLengthValidator(8);

console.log(isLongEnough("password"));
```

## Declarative Data Flow

Declarative code describes what should happen.

```javascript
const activeUserNames = users
  .filter((user) => user.active)
  .map((user) => user.name)
  .sort();
```

This is often clearer than manually managing indexes and temporary variables.

## When a Loop Is Better

Functional methods are useful, but a plain loop can be clearer for complex
control flow.

```javascript
function findFirstInvalidUser(users) {
  for (const user of users) {
    if (!user.email || !user.email.includes("@")) {
      return user;
    }
  }

  return null;
}
```

## Real Pain Points

- Long chains can hide too much logic. Extract named helper functions when the
  chain becomes hard to read.
- `reduce` is powerful, but it is not always the clearest tool. Use it when it
  makes the intent obvious.
- Functional style does not automatically mean pure or simple. Side effects can
  still hide inside callbacks.

## Practice

1. Use `map` to extract names from users.
2. Use `filter` to keep active users.
3. Use `reduce` to calculate a total.
4. Create a validator factory that returns a validation function.

## Related Topics

- [Pure Functions](pure_functions.md)
- [Arrays](../03_data_structures/arrays.md)
- [Closures](../02_core_concepts/closures.md)

