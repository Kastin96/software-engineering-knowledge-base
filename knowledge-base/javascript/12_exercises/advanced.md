# Advanced Exercises

## 1. Search Result State Manager

Write a small state manager for search results.

Requirements:

- keep `query`, `status`, `results`, and `error`;
- expose `setQuery`, `startLoading`, `setSuccess`, and `setError`;
- never mutate the previous state object;
- keep the functions pure.

Initial state:

```javascript
const initialSearchState = {
  query: "",
  status: "idle",
  results: [],
  error: null,
};
```

## 2. Stale Request Guard

Write a function `createLatestOnlyRunner()` for search requests.

Requirements:

- it accepts async operations;
- only the latest operation result should be used;
- older completed operations should return `null`.

Usage:

```javascript
const runLatest = createLatestOnlyRunner();

const result = await runLatest(() => searchProducts("keyboard"));
```

## 3. Config Reader

Write a function `createConfig(env)` that reads configuration from an object
like `process.env`.

Rules:

- `DATABASE_URL` is required;
- `PORT` defaults to `3000`;
- `ENABLE_LOGS` is true only when the value is `"true"`;
- throw useful errors for invalid required values.

## 4. Event Emitter

Write a small event emitter.

Requirements:

- `on(eventName, listener)` subscribes;
- `off(eventName, listener)` unsubscribes;
- `emit(eventName, payload)` calls listeners;
- `on` returns an unsubscribe function;
- one listener throwing should not stop the others.

## 5. In-Memory Repository

Write an in-memory user repository.

Requirements:

- `create(user)` creates a user with a generated id;
- `findById(id)` returns a user or `null`;
- `update(id, updates)` updates a user or returns `null`;
- `delete(id)` removes a user and returns `true` or `false`;
- callers should not be able to mutate stored data directly.

## 6. Request Queue With Concurrency Limit

Write a function `runWithConcurrencyLimit(tasks, limit)` where:

- `tasks` is an array of functions that return promises;
- no more than `limit` tasks run at the same time;
- results keep the same order as input tasks;
- if a task fails, the returned promise rejects.

Usage:

```javascript
const results = await runWithConcurrencyLimit(
  [
    () => fetchJson("/api/1"),
    () => fetchJson("/api/2"),
    () => fetchJson("/api/3"),
  ],
  2,
);
```

## 7. Form Validation Engine

Write a reusable validation engine.

Requirements:

- field rules are functions;
- each rule returns `null` or an error message;
- validation returns an object of field errors;
- support multiple rules per field.

Example rule config:

```javascript
const rules = {
  email: [required("Email is required"), email("Email is invalid")],
  password: [minLength(12, "Password must be at least 12 characters")],
};
```

## 8. Normalized Store

Write helpers for normalized data.

State shape:

```javascript
const state = {
  ids: [],
  entities: {},
};
```

Requirements:

- `addOne(state, item)` adds an item;
- `updateOne(state, id, updates)` updates an item;
- `removeOne(state, id)` removes an item;
- keep updates immutable;
- avoid duplicate ids.

