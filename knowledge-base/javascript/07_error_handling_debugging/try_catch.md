# try...catch

## Goal

Understand how to handle errors intentionally with `try...catch`.

## Why It Matters

Some failures are expected: invalid JSON, failed network requests, missing
configuration, invalid user input, or unavailable browser APIs. `try...catch`
lets you handle those failures without crashing the whole flow.

## Basic Syntax

```javascript
try {
  const data = JSON.parse('{"name":"Alex"}');
  console.log(data.name);
} catch (error) {
  console.log("Failed to parse JSON:", error.message);
}
```

Code inside `catch` runs only if code inside `try` throws.

## Handling Invalid JSON

```javascript
function parseJsonOrNull(value) {
  try {
    return JSON.parse(value);
  } catch {
    return null;
  }
}

console.log(parseJsonOrNull('{"name":"Alex"}'));
console.log(parseJsonOrNull("{bad json}"));
```

## Using finally

`finally` runs after `try` or `catch`. It is useful for cleanup.

```javascript
let loading = true;

try {
  console.log("Load data");
} catch (error) {
  console.log(error.message);
} finally {
  loading = false;
}

console.log(loading);
```

## Rethrowing Errors

Sometimes you should add context and throw the error again.

```javascript
function parseUser(json) {
  try {
    return JSON.parse(json);
  } catch (error) {
    throw new Error(`Could not parse user data: ${error.message}`);
  }
}
```

This lets a higher-level function decide what to do with the failure.

## Async try...catch

Use `try...catch` with `await`.

```javascript
async function loadUser(id) {
  const response = await fetch(`/api/users/${id}`);

  if (!response.ok) {
    throw new Error(`Request failed with status ${response.status}`);
  }

  return response.json();
}

async function run() {
  try {
    const user = await loadUser(1);
    console.log(user);
  } catch (error) {
    console.log("Failed to load user:", error.message);
  }
}
```

## Boundary Handling

Handle errors at a boundary: UI event handler, API route handler, CLI command,
or app startup.

```javascript
async function handleSubmit(event) {
  event.preventDefault();

  try {
    await saveForm();
    showMessage("Saved");
  } catch (error) {
    showMessage("Could not save the form");
    console.error(error);
  }
}
```

Lower-level functions can throw. Boundary code can decide what the user sees.

## Real Pain Points

- Catching an error and doing nothing makes debugging much harder.

```javascript
try {
  await saveUser(user);
} catch (error) {
  console.error("Failed to save user:", error);
  throw error;
}
```

- `try...catch` does not catch a promise rejection unless you `await` the
  promise inside the `try` block.

```javascript
try {
  await loadUser(1);
} catch (error) {
  console.log(error.message);
}
```

- Returning fake success from `catch` can hide real data problems.

```javascript
function parseSettings(json) {
  try {
    return JSON.parse(json);
  } catch {
    return {
      theme: "light",
    };
  }
}
```

Use fallback values only when a fallback is truly valid for the feature.

## Practice

1. Wrap `JSON.parse` in `try...catch`.
2. Add `finally` to reset a loading flag.
3. Rethrow an error with extra context.
4. Handle an async failure with `await` inside `try`.

## Related Topics

- [Errors](errors.md)
- [Async Error Handling](../04_async_javascript/error_handling_async.md)
- [Console](console.md)

