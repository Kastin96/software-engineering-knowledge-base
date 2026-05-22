# API Client

## Goal

Create a small reusable API client with JSON handling, HTTP errors, query
parameters, and request cancellation.

## Example: Minimal JSON Client

```javascript
class ApiError extends Error {
  constructor(message, { status, body } = {}) {
    super(message);
    this.name = "ApiError";
    this.status = status;
    this.body = body;
  }
}

function buildUrl(baseUrl, path, query) {
  const url = new URL(path, baseUrl);

  for (const [key, value] of Object.entries(query ?? {})) {
    if (value !== undefined && value !== null && value !== "") {
      url.searchParams.set(key, String(value));
    }
  }

  return url.toString();
}

async function parseJsonResponse(response) {
  const text = await response.text();

  if (!text) {
    return null;
  }

  try {
    return JSON.parse(text);
  } catch (error) {
    throw new ApiError("Response is not valid JSON", {
      status: response.status,
      body: text,
    });
  }
}

function createApiClient({ baseUrl, getToken }) {
  async function request(path, options = {}) {
    const {
      method = "GET",
      query,
      body,
      signal,
      headers = {},
    } = options;

    const token = getToken?.();

    const response = await fetch(buildUrl(baseUrl, path, query), {
      method,
      signal,
      headers: {
        Accept: "application/json",
        ...(body ? { "Content-Type": "application/json" } : {}),
        ...(token ? { Authorization: `Bearer ${token}` } : {}),
        ...headers,
      },
      body: body ? JSON.stringify(body) : undefined,
    });

    const data = await parseJsonResponse(response);

    if (!response.ok) {
      throw new ApiError(`Request failed with status ${response.status}`, {
        status: response.status,
        body: data,
      });
    }

    return data;
  }

  return {
    get(path, options) {
      return request(path, { ...options, method: "GET" });
    },
    post(path, body, options) {
      return request(path, { ...options, method: "POST", body });
    },
    patch(path, body, options) {
      return request(path, { ...options, method: "PATCH", body });
    },
    delete(path, options) {
      return request(path, { ...options, method: "DELETE" });
    },
  };
}
```

## Usage

```javascript
const api = createApiClient({
  baseUrl: "https://api.example.com",
  getToken() {
    return localStorage.getItem("accessToken");
  },
});

const users = await api.get("/users", {
  query: {
    page: 1,
    search: "alex",
  },
});

const createdUser = await api.post("/users", {
  name: "Alex",
  email: "alex@example.com",
});

console.log(users);
console.log(createdUser);
```

## Cancellation

```javascript
const controller = new AbortController();

const request = api.get("/search", {
  query: { q: "javascript" },
  signal: controller.signal,
});

controller.abort();

try {
  await request;
} catch (error) {
  if (error.name === "AbortError") {
    console.log("Request cancelled");
  } else {
    throw error;
  }
}
```

## Handling API Errors

```javascript
try {
  await api.get("/users/missing");
} catch (error) {
  if (error instanceof ApiError && error.status === 404) {
    console.log("User not found");
  } else {
    console.error("Unexpected API error", error);
  }
}
```

## What This Demonstrates

- Centralized `fetch` behavior.
- Consistent JSON parsing.
- Proper `response.ok` handling.
- Query string construction with `URL`.
- Optional auth header.
- Request cancellation with `AbortController`.

## Practice

1. Add a `put` method.
2. Add a timeout option using `AbortController`.
3. Add a `requestId` header for debugging.
4. Normalize API errors into user-friendly messages.

## Related Topics

- [Fetch](../04_async_javascript/fetch.md)
- [Fetch in the Browser](../05_browser_javascript/fetch_in_browser.md)
- [Async Error Handling](../04_async_javascript/error_handling_async.md)

