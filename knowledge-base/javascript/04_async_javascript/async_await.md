# Async and Await

## Goal

Understand how to write promise-based code with `async` and `await`.

## Why It Matters

`async` and `await` make asynchronous code look closer to normal step-by-step
code. This is the most common style in modern JavaScript projects.

## Explanation

An `async` function always returns a promise.

```javascript
async function getMessage() {
  return "Hello";
}

getMessage().then((message) => {
  console.log(message);
});
```

`await` waits for a promise inside an async function.

```javascript
function loadUser() {
  return Promise.resolve({ id: 1, name: "Alex" });
}

async function run() {
  const user = await loadUser();
  console.log(user.name);
}

run();
```

## Error Handling

Use `try...catch` with `await`.

```javascript
async function loadUser(id) {
  if (!id) {
    throw new Error("User id is required");
  }

  return { id, name: "Alex" };
}

async function run() {
  try {
    const user = await loadUser(null);
    console.log(user);
  } catch (error) {
    console.log(error.message);
  }
}

run();
```

## Sequential Work

Use sequential `await` when the second operation depends on the first.

```javascript
async function loadUser() {
  return { id: 1, name: "Alex" };
}

async function loadOrders(userId) {
  return [`Order for user ${userId}`];
}

async function run() {
  const user = await loadUser();
  const orders = await loadOrders(user.id);

  console.log(orders);
}

run();
```

## Parallel Work

Use `Promise.all` when operations are independent.

```javascript
async function loadProfile() {
  return { name: "Alex" };
}

async function loadSettings() {
  return { theme: "dark" };
}

async function run() {
  const [profile, settings] = await Promise.all([
    loadProfile(),
    loadSettings(),
  ]);

  console.log(profile);
  console.log(settings);
}

run();
```

## Returning Values

Returning from an async function resolves its promise.

```javascript
async function calculateTotal() {
  return 100;
}

async function run() {
  const total = await calculateTotal();
  console.log(total);
}

run();
```

Throwing inside an async function rejects its promise.

```javascript
async function fail() {
  throw new Error("Something failed");
}

fail().catch((error) => {
  console.log(error.message);
});
```

## Real Pain Points

- Forgetting `await` gives you a promise instead of the resolved value.

```javascript
async function run() {
  const userPromise = loadUser();
  const user = await userPromise;

  console.log(user.name);
}
```

- Using `await` in a loop can be slow when operations are independent.

```javascript
async function loadUsers(ids) {
  const requests = ids.map((id) => loadUser(id));
  return Promise.all(requests);
}
```

- `try...catch` only catches awaited promises inside the `try` block.

```javascript
async function run() {
  try {
    await loadUser(null);
  } catch (error) {
    console.log(error.message);
  }
}
```

## Practice

1. Write an async function that returns a string.
2. Await a promise returned by another function.
3. Wrap an awaited call in `try...catch`.
4. Use `Promise.all` with `await` for two independent calls.

## Related Topics

- [Promises](promises.md)
- [Fetch](fetch.md)
- [Async Error Handling](error_handling_async.md)

