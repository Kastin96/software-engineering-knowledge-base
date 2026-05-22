# Common Antipatterns

## Goal

Recognize code shapes that often lead to bugs or maintenance problems.

## Why It Matters

An antipattern is a familiar-looking solution that usually creates more problems
over time. Recognizing antipatterns helps you refactor earlier and review code
more effectively.

## Hidden Global State

Global mutable state can make behavior depend on call order.

```javascript
let currentUser = null;

function setCurrentUser(user) {
  currentUser = user;
}

function canEditProject() {
  return currentUser?.role === "admin";
}
```

Prefer passing needed data explicitly when possible.

```javascript
function canEditProject(user) {
  return user?.role === "admin";
}
```

## Large Functions

A function that reads input, validates, transforms, saves, logs, and updates UI
is doing too much.

```javascript
async function handleSubmit(event) {
  event.preventDefault();

  const input = readForm(event.currentTarget);
  const errors = validateInput(input);

  if (Object.keys(errors).length > 0) {
    showErrors(errors);
    return;
  }

  const payload = buildPayload(input);
  await saveUser(payload);
  showSuccess();
}
```

This version is still one workflow, but the details are split into named
helpers.

## Boolean Flags That Change Behavior Too Much

Boolean flags can make one function contain several different functions.

```javascript
function formatUser(user, includeEmail) {
  if (includeEmail) {
    return `${user.name} <${user.email}>`;
  }

  return user.name;
}
```

Separate functions can be clearer.

```javascript
function formatUserName(user) {
  return user.name;
}

function formatUserWithEmail(user) {
  return `${user.name} <${user.email}>`;
}
```

## Mutation Across Boundaries

Changing data that was passed into a function can surprise callers.

```javascript
function deactivateUser(user) {
  user.active = false;
  return user;
}
```

Prefer returning a new value when callers expect original data to stay stable.

```javascript
function deactivateUser(user) {
  return {
    ...user,
    active: false,
  };
}
```

## Swallowing Errors

Silently catching errors hides production problems.

```javascript
try {
  await saveUser(user);
} catch {
  // Nothing happens here.
}
```

At least log context or return a meaningful failure.

```javascript
try {
  await saveUser(user);
} catch (error) {
  console.error("Failed to save user", error);
  throw error;
}
```

## Over-Abstraction

An abstraction should remove real duplication or clarify intent. If it hides a
simple operation behind several layers, it may make the code harder to change.

```javascript
function getUserDisplayName(user) {
  return user.name;
}
```

This helper may be useful if display-name rules will grow. If it only returns a
property forever, direct access may be clearer.

## Real Pain Points

- Antipatterns are context-dependent. A pattern that is harmful in one place can
  be acceptable in a small script.
- Refactor toward clarity, not toward a pattern checklist.
- The most expensive antipatterns usually hide data flow, errors, or side
  effects.

## Practice

1. Refactor a function that depends on global state.
2. Split one large function into named helper functions.
3. Replace a boolean flag with two clearer functions.
4. Change a mutating function so it returns a new object.

## Related Topics

- [Clean Code](clean_code.md)
- [Immutability](immutability.md)
- [Error Handling and Debugging](../07_error_handling_debugging/README.md)
