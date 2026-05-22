# Errors

## Goal

Understand what JavaScript errors are and how to read them.

## Why It Matters

Errors are not just noise. They are signals that tell you what failed, where it
failed, and often why it failed. Reading errors well is one of the fastest ways
to improve as a developer.

## Explanation

An error is an object that describes a failure.

```javascript
const error = new Error("Something went wrong");

console.log(error.message);
```

You can throw an error when code cannot continue safely.

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }

  return a / b;
}

console.log(divide(10, 2));
```

## Error Message

The message should explain what failed in a way that helps the caller.

```javascript
function getUserEmail(user) {
  if (!user.email) {
    throw new Error("User email is required");
  }

  return user.email;
}
```

## Stack Trace

A stack trace shows where the error happened and which functions were called.

```javascript
function parseUser(json) {
  return JSON.parse(json);
}

function loadUser() {
  return parseUser("{bad json}");
}

loadUser();
```

When this fails, the stack trace helps you follow the path to the failure.

## Common Built-In Error Types

`Error` is the general error type.

```javascript
throw new Error("Generic failure");
```

`TypeError` usually means a value is not the expected type.

```javascript
const user = null;

// TypeError: Cannot read properties of null
// console.log(user.name);
```

`ReferenceError` usually means a variable name does not exist in the current
scope.

```javascript
// ReferenceError
// console.log(missingValue);
```

`SyntaxError` means the code or parsed text has invalid syntax.

```javascript
// SyntaxError from invalid JSON
// JSON.parse("{bad json}");
```

## Custom Error Names

For simple code, `Error` is usually enough. For larger codebases, custom error
classes can help identify error categories.

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

function createUser(input) {
  if (!input.email) {
    throw new ValidationError("Email is required");
  }

  return {
    email: input.email,
  };
}
```

## Real Pain Points

- Error messages without context slow down debugging.

```javascript
throw new Error("Invalid user payload: email is required");
```

- Catching and replacing an error can hide the original cause. Preserve useful
  context when possible.

```javascript
try {
  JSON.parse("{bad json}");
} catch (error) {
  throw new Error(`Failed to parse user JSON: ${error.message}`);
}
```

- Not every failure should be handled where it happens. Sometimes lower-level
  code should throw and higher-level code should decide what to show the user.

## Practice

1. Throw an error when a required function argument is missing.
2. Read a stack trace and identify the first line from your code.
3. Create a custom `ValidationError`.
4. Rewrite a vague error message into a more useful one.

## Related Topics

- [try...catch](try_catch.md)
- [Debugging](debugging.md)
- [Async Error Handling](../04_async_javascript/error_handling_async.md)

