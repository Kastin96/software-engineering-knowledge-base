# Fetch in the Browser

## Goal

Understand practical browser-specific behavior when using `fetch`.

## Why It Matters

Browser requests are affected by HTTP status codes, CORS, credentials,
cancellation, page navigation, and stale UI updates.

## Basic Request

```javascript
async function loadUsers() {
  const response = await fetch("/api/users");

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`);
  }

  return response.json();
}
```

Relative URLs are resolved against the current page origin.

## Loading State

```javascript
async function renderUsers() {
  const status = document.querySelector("#status");
  const list = document.querySelector("#users");

  status.textContent = "Loading...";

  try {
    const users = await loadUsers();

    list.innerHTML = "";

    for (const user of users) {
      const item = document.createElement("li");
      item.textContent = user.name;
      list.append(item);
    }

    status.textContent = "";
  } catch (error) {
    status.textContent = "Failed to load users";
  }
}
```

## Sending JSON

```javascript
async function createUser(user) {
  const response = await fetch("/api/users", {
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
```

## Credentials

Cookies are controlled with the `credentials` option.

```javascript
await fetch("/api/profile", {
  credentials: "include",
});
```

Use this only when the API and security requirements need cookies to be sent.

## CORS

CORS means Cross-Origin Resource Sharing. The browser blocks some requests to
different origins unless the server allows them.

```javascript
await fetch("https://api.example.com/users");
```

If this fails because of CORS, the fix is usually on the server configuration,
not in frontend JavaScript.

## Aborting Requests

```javascript
const controller = new AbortController();

async function loadSearchResults(query) {
  const response = await fetch(`/api/search?q=${encodeURIComponent(query)}`, {
    signal: controller.signal,
  });

  return response.json();
}

controller.abort();
```

## Avoiding Stale UI Updates

```javascript
let latestSearchId = 0;

async function search(query) {
  const searchId = latestSearchId + 1;
  latestSearchId = searchId;

  const response = await fetch(`/api/search?q=${encodeURIComponent(query)}`);
  const results = await response.json();

  if (searchId !== latestSearchId) {
    return;
  }

  renderResults(results);
}
```

## Real Pain Points

- `fetch` does not reject for HTTP `400` or `500`; check `response.ok`.
- CORS errors cannot usually be fixed only from client code.
- Search and autocomplete can show stale results if older requests finish after
  newer requests.
- Cookie-based authentication may require the right `credentials` option and
  server-side CORS settings.

## Practice

1. Write a browser `fetch` helper that checks `response.ok`.
2. Render loading, success, and error states.
3. Send JSON with a POST request.
4. Add a stale-request guard to a search function.

## Related Topics

- [Fetch](../04_async_javascript/fetch.md)
- [Async Error Handling](../04_async_javascript/error_handling_async.md)
- [DOM](dom.md)

