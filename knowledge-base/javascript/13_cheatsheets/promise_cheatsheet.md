# Promise Cheatsheet

## Promise States

- `pending`: not finished yet.
- `fulfilled`: completed successfully.
- `rejected`: failed.

## Create Resolved or Rejected Promise

```javascript
Promise.resolve(value);
Promise.reject(new Error("Failed"));
```

## Basic Chain

```javascript
loadUser()
  .then((user) => loadOrders(user.id))
  .then((orders) => {
    console.log(orders);
  })
  .catch((error) => {
    console.error(error);
  });
```

Return promises from `.then()` when the next step depends on them.

## async/await

```javascript
async function run() {
  try {
    const user = await loadUser();
    const orders = await loadOrders(user.id);
    return orders;
  } catch (error) {
    console.error(error);
    throw error;
  }
}
```

## Sequential vs Parallel

Sequential when step two needs step one:

```javascript
const user = await loadUser();
const orders = await loadOrders(user.id);
```

Parallel when independent:

```javascript
const [profile, settings] = await Promise.all([
  loadProfile(),
  loadSettings(),
]);
```

## Promise.all

All must succeed.

```javascript
const results = await Promise.all([taskA(), taskB(), taskC()]);
```

If one rejects, the whole `Promise.all` rejects.

## Promise.allSettled

Waits for all results, success or failure.

```javascript
const results = await Promise.allSettled([taskA(), taskB()]);

const successful = results
  .filter((result) => result.status === "fulfilled")
  .map((result) => result.value);

const failed = results
  .filter((result) => result.status === "rejected")
  .map((result) => result.reason);
```

## Promise.race

Settles when the first promise settles.

```javascript
const result = await Promise.race([request(), timeout()]);
```

## Promise.any

Fulfills when the first promise fulfills. Rejects only if all reject.

```javascript
const result = await Promise.any([primaryRequest(), fallbackRequest()]);
```

## Timeout Helper

```javascript
function wait(milliseconds) {
  return new Promise((resolve) => {
    setTimeout(resolve, milliseconds);
  });
}
```

Reject after timeout:

```javascript
function timeout(milliseconds) {
  return new Promise((_, reject) => {
    setTimeout(() => {
      reject(new Error(`Timed out after ${milliseconds}ms`));
    }, milliseconds);
  });
}
```

## Retry

```javascript
async function retry(operation, attempts) {
  let lastError;

  for (let attempt = 1; attempt <= attempts; attempt += 1) {
    try {
      return await operation();
    } catch (error) {
      lastError = error;
    }
  }

  throw lastError;
}
```

## Fetch Pattern

```javascript
async function requestJson(url, options) {
  const response = await fetch(url, options);

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`);
  }

  return response.json();
}
```

## AbortController

```javascript
const controller = new AbortController();

const request = fetch("/api/search", {
  signal: controller.signal,
});

controller.abort();
```

## Stale Request Guard

```javascript
let latestRequestId = 0;

async function runLatest(operation) {
  const requestId = latestRequestId + 1;
  latestRequestId = requestId;

  const result = await operation();

  if (requestId !== latestRequestId) {
    return null;
  }

  return result;
}
```

## Error Handling Rules

- Use `try...catch` around awaited operations.
- Check `response.ok` for HTTP errors.
- Do not swallow errors silently.
- Add context before rethrowing when it helps debugging.
- Run independent operations with `Promise.all`.

