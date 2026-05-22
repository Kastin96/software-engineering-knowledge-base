# Fetch

## Goal

Understand how to make HTTP requests with `fetch`.

## Why It Matters

Frontend apps and many JavaScript services communicate with APIs. `fetch` is a
standard way to request data over HTTP.

## Basic GET Request

`fetch` returns a promise.

```javascript
async function loadUsers() {
  const response = await fetch("https://example.com/api/users");
  const users = await response.json();

  return users;
}
```

`response.json()` also returns a promise, so it needs `await`.

## Checking Response Status

`fetch` only rejects for network-level failures. It does not reject just because
the server returns `404` or `500`.

```javascript
async function loadUser(id) {
  const response = await fetch(`https://example.com/api/users/${id}`);

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`);
  }

  return response.json();
}
```

## POST Request

```javascript
async function createUser(user) {
  const response = await fetch("https://example.com/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(user),
  });

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`);
  }

  return response.json();
}

createUser({
  name: "Alex",
  email: "alex@example.com",
});
```

## Query Parameters

Use `URLSearchParams` to build query strings.

```javascript
const params = new URLSearchParams({
  page: "1",
  pageSize: "20",
});

const url = `https://example.com/api/users?${params.toString()}`;

console.log(url);
```

## Abort a Request

Use `AbortController` to cancel a request.

```javascript
const controller = new AbortController();

async function loadData() {
  const response = await fetch("https://example.com/api/data", {
    signal: controller.signal,
  });

  return response.json();
}

controller.abort();
```

This is useful when a user navigates away or starts a newer request.

## Real Pain Points

- Forgetting to check `response.ok` treats HTTP errors like successful responses.
- Forgetting to `await response.json()` leaves you with a promise.
- A request can finish after the UI no longer needs it. Cancellation or stale
  result checks can prevent outdated data from replacing fresh data.

```javascript
async function safeLoadUser(id) {
  const response = await fetch(`https://example.com/api/users/${id}`);

  if (!response.ok) {
    throw new Error(`Failed to load user ${id}`);
  }

  return response.json();
}
```

## Practice

1. Write a GET request function.
2. Add a `response.ok` check.
3. Write a POST request with a JSON body.
4. Build a URL with query parameters.

## Related Topics

- [Async and Await](async_await.md)
- [JSON](../03_data_structures/json.md)
- [Async Error Handling](error_handling_async.md)

