# Async Error Handling

## Goal

Understand how to handle failures in promise-based and `async`/`await` code.

## Why It Matters

Network requests fail, servers return errors, user input can be invalid, and
async operations can finish in unexpected order. Good async error handling makes
apps more reliable and easier to debug.

## Promise Errors

Use `.catch()` for promise chains.

```javascript
function loadUser(id) {
  if (!id) {
    return Promise.reject(new Error("User id is required"));
  }

  return Promise.resolve({ id, name: "Alex" });
}

loadUser(null)
  .then((user) => {
    console.log(user);
  })
  .catch((error) => {
    console.log(error.message);
  });
```

## Async and Await Errors

Use `try...catch` around awaited code.

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

## Handling HTTP Errors

With `fetch`, check `response.ok` yourself.

```javascript
async function requestJson(url) {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }

  return response.json();
}
```

## Using finally

`finally` runs after success or failure. It is useful for cleanup.

```javascript
async function loadData() {
  let loading = true;

  try {
    const data = await requestJson("https://example.com/api/data");
    return data;
  } catch (error) {
    console.log(error.message);
    return null;
  } finally {
    loading = false;
    console.log("loading:", loading);
  }
}
```

## Rethrowing Errors

Sometimes a lower-level function should add context and rethrow.

```javascript
async function loadUserProfile(userId) {
  try {
    return await requestJson(`https://example.com/api/users/${userId}`);
  } catch (error) {
    throw new Error(`Could not load profile for user ${userId}: ${error.message}`);
  }
}
```

## Avoiding Stale Results

Async operations can finish in a different order than they started.

```javascript
let latestRequestId = 0;

async function search(query) {
  const requestId = latestRequestId + 1;
  latestRequestId = requestId;

  const response = await fetch(`/api/search?q=${encodeURIComponent(query)}`);
  const results = await response.json();

  if (requestId !== latestRequestId) {
    return null;
  }

  return results;
}
```

This prevents an older request from overwriting newer results.

## Real Pain Points

- Handling only network errors and forgetting HTTP status errors.
- Catching an error too early and hiding useful context from the caller.
- Starting multiple requests and letting an older response overwrite newer UI data.
- Forgetting cleanup logic for loading states, intervals, or abort controllers.

```javascript
async function loadPageData() {
  try {
    return await requestJson("/api/page");
  } catch (error) {
    console.log("Failed to load page data:", error.message);
    throw error;
  }
}
```

## Practice

1. Wrap an awaited function call in `try...catch`.
2. Add `response.ok` handling to a fetch helper.
3. Use `finally` to reset a loading flag.
4. Write a stale-request guard for a search function.

## Related Topics

- [Promises](promises.md)
- [Async and Await](async_await.md)
- [Fetch](fetch.md)
