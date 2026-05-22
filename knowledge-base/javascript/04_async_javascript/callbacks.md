# Callbacks

## Goal

Understand how functions can be passed to other functions and called later.

## Why It Matters

Callbacks are the older foundation of async JavaScript. They still appear in
event listeners, timers, array methods, Node.js APIs, and many libraries.

## Explanation

A callback is a function passed as an argument to another function.

```javascript
function greet(name, callback) {
  const message = `Hello, ${name}`;
  callback(message);
}

greet("Alex", (message) => {
  console.log(message);
});
```

The receiving function decides when to call the callback.

## Callback With Timer

```javascript
function runLater(callback) {
  setTimeout(() => {
    callback("Done");
  }, 1000);
}

runLater((result) => {
  console.log(result);
});
```

## Callback With Array Methods

Array methods use callbacks for synchronous transformations.

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map((number) => {
  return number * 2;
});

console.log(doubled); // [2, 4, 6]
```

## Error-First Callbacks

Some Node.js APIs use an error-first callback style.

```javascript
function readUser(id, callback) {
  if (!id) {
    callback(new Error("User id is required"));
    return;
  }

  callback(null, { id, name: "Alex" });
}

readUser(1, (error, user) => {
  if (error) {
    console.log(error.message);
    return;
  }

  console.log(user);
});
```

The first argument is the error. If there is no error, it is usually `null`.

## Callback Nesting

Callbacks become hard to read when many async steps depend on each other.

```javascript
getUser(1, (userError, user) => {
  if (userError) {
    console.log(userError.message);
    return;
  }

  getOrders(user.id, (ordersError, orders) => {
    if (ordersError) {
      console.log(ordersError.message);
      return;
    }

    console.log(orders);
  });
});
```

Promises and `async`/`await` are usually better for this kind of flow.

## Real Pain Points

- Deeply nested callbacks are hard to read and test.
- Forgetting to return after handling an error can continue the function by accident.
- A callback can be called more than once if the function is written poorly.

```javascript
function loadUser(id, callback) {
  if (!id) {
    callback(new Error("Missing id"));
    return;
  }

  callback(null, { id, name: "Alex" });
}
```

## Practice

1. Write a function that accepts a value and a callback.
2. Use `setTimeout` to call a callback after one second.
3. Write an error-first callback example.
4. Rewrite a nested callback flow as notes for a future promise version.

## Related Topics

- [Timers](timers.md)
- [Promises](promises.md)
- [Async Error Handling](error_handling_async.md)

