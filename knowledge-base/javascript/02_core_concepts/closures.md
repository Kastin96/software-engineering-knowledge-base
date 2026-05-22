# Closures

## Goal

Understand how a function can remember variables from the scope where it was
created.

## Why It Matters

Closures are used in callbacks, event handlers, factory functions, module
patterns, and many real JavaScript APIs.

## Explanation

A closure happens when a function remembers variables from its outer scope, even
after the outer function has finished running.

```javascript
function createGreeting(name) {
  return function () {
    return `Hello, ${name}`;
  };
}

const greetAlex = createGreeting("Alex");

console.log(greetAlex()); // Hello, Alex
```

The inner function remembers `name`.

## Simple Counter Example

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count += 1;
    return count;
  };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

The variable `count` is not global. It is protected inside `createCounter`, but
the returned function can still use it.

## Returning Multiple Functions

Closures can expose several functions that share the same private state.

```javascript
function createScore() {
  let points = 0;

  return {
    add(value) {
      points += value;
    },
    get() {
      return points;
    },
    reset() {
      points = 0;
    },
  };
}

const score = createScore();

score.add(10);
score.add(5);

console.log(score.get()); // 15

score.reset();

console.log(score.get()); // 0
```

## Closures in Loops

With `let`, each loop step gets its own block-scoped variable.

```javascript
const actions = [];

for (let index = 0; index < 3; index += 1) {
  actions.push(function () {
    return index;
  });
}

console.log(actions[0]()); // 0
console.log(actions[1]()); // 1
console.log(actions[2]()); // 2
```

## Practical Use Case

Create a function with configuration.

```javascript
function createLogger(prefix) {
  return function (message) {
    console.log(`[${prefix}] ${message}`);
  };
}

const logApi = createLogger("API");
const logAuth = createLogger("AUTH");

logApi("Request started");
logAuth("User logged in");
```

## Common Mistakes

- Thinking the outer function's variables disappear completely:

```javascript
function createNameReader() {
  const name = "Alex";

  return function () {
    return name;
  };
}

const readName = createNameReader();

console.log(readName()); // Alex
```

- Using global variables when a closure would be cleaner:

```javascript
function createIdGenerator() {
  let currentId = 0;

  return function () {
    currentId += 1;
    return currentId;
  };
}

const nextId = createIdGenerator();

console.log(nextId()); // 1
console.log(nextId()); // 2
```

## Practice

1. Create a counter that starts from a custom number.
2. Create a logger that remembers a prefix.
3. Create a function that stores a private username and returns a function to read it.
4. Explain why closure state is not the same as a global variable.

## Related Topics

- [Scope](scope.md)
- [Functions](../01_language_basics/functions.md)
- [Modules](modules.md)

