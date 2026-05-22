# Promises

## Goal

Understand how promises represent values that may be available later.

## Why It Matters

Promises are the foundation of modern async JavaScript. `fetch`, many browser
APIs, many Node.js APIs, and `async`/`await` all use promises.

## Explanation

A promise represents a future result. It can be:

- pending;
- fulfilled;
- rejected.

```javascript
const promise = Promise.resolve("Done");

promise.then((value) => {
  console.log(value);
});
```

## Creating a Promise

```javascript
function wait(milliseconds) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(`Waited ${milliseconds}ms`);
    }, milliseconds);
  });
}

wait(1000).then((message) => {
  console.log(message);
});
```

Use `reject` for failures.

```javascript
function loadUser(id) {
  return new Promise((resolve, reject) => {
    if (!id) {
      reject(new Error("User id is required"));
      return;
    }

    resolve({ id, name: "Alex" });
  });
}

loadUser(1)
  .then((user) => {
    console.log(user);
  })
  .catch((error) => {
    console.log(error.message);
  });
```

## Chaining

`then` returns a new promise, so you can chain steps.

```javascript
loadUser(1)
  .then((user) => {
    return user.name;
  })
  .then((name) => {
    console.log(name.toUpperCase());
  })
  .catch((error) => {
    console.log(error.message);
  });
```

## Promise.all

Use `Promise.all` when independent async operations can run in parallel and all
results are required.

```javascript
function loadProfile() {
  return Promise.resolve({ name: "Alex" });
}

function loadSettings() {
  return Promise.resolve({ theme: "dark" });
}

Promise.all([loadProfile(), loadSettings()])
  .then(([profile, settings]) => {
    console.log(profile);
    console.log(settings);
  })
  .catch((error) => {
    console.log(error.message);
  });
```

If one promise rejects, `Promise.all` rejects.

## Promise.allSettled

Use `Promise.allSettled` when you want every result, even if some fail.

```javascript
const requests = [
  Promise.resolve("first"),
  Promise.reject(new Error("second failed")),
  Promise.resolve("third"),
];

Promise.allSettled(requests).then((results) => {
  console.log(results);
});
```

## Real Pain Points

- Not returning a promise inside a chain breaks the sequence.

```javascript
loadUser(1)
  .then((user) => {
    return loadOrders(user.id);
  })
  .then((orders) => {
    console.log(orders);
  });
```

- Missing `.catch()` can hide failures or produce unhandled rejections.

```javascript
loadUser(null).catch((error) => {
  console.log(error.message);
});
```

- Running independent promises one after another makes code slower than needed.

```javascript
const profilePromise = loadProfile();
const settingsPromise = loadSettings();

Promise.all([profilePromise, settingsPromise]).then(([profile, settings]) => {
  console.log(profile, settings);
});
```

## Practice

1. Create a promise that resolves after one second.
2. Create a promise that rejects when an id is missing.
3. Chain two `.then()` calls.
4. Use `Promise.all` for two independent async functions.

## Related Topics

- [Async and Await](async_await.md)
- [Timers](timers.md)
- [Async Error Handling](error_handling_async.md)

