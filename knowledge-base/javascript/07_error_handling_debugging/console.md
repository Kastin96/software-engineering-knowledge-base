# Console

## Goal

Use `console` methods to inspect code clearly during development.

## Why It Matters

The console is the fastest debugging tool for many small problems. Good logs can
show values, flow, timing, and failures without adding much setup.

## console.log

Use `console.log` to print values.

```javascript
const user = {
  id: 1,
  name: "Alex",
};

console.log(user);
```

Prefer labeled logs when printing multiple values.

```javascript
console.log("current user", user);
console.log("user id", user.id);
```

## console.error

Use `console.error` for failures.

```javascript
try {
  JSON.parse("{bad json}");
} catch (error) {
  console.error("Failed to parse settings", error);
}
```

## console.warn

Use `console.warn` for suspicious situations that are not fatal.

```javascript
function createUser(input) {
  if (!input.role) {
    console.warn("User role missing, using viewer role");
  }

  return {
    name: input.name,
    role: input.role ?? "viewer",
  };
}
```

## console.table

Use `console.table` for arrays of objects.

```javascript
const users = [
  { id: 1, name: "Alex", active: true },
  { id: 2, name: "Sam", active: false },
];

console.table(users);
```

## console.time

Use `console.time` and `console.timeEnd` to measure rough timing.

```javascript
console.time("calculate");

const total = [1, 2, 3, 4, 5].reduce((sum, value) => sum + value, 0);

console.timeEnd("calculate");
console.log(total);
```

## console.group

Use groups to organize related logs.

```javascript
console.group("User validation");
console.log("input", { name: "", email: "bad" });
console.log("errors", { name: "Name is required" });
console.groupEnd();
```

## Logging Objects Safely

Objects can change after you log them, especially in browser DevTools. If you
need a snapshot, log a copied value.

```javascript
const snapshot = JSON.parse(JSON.stringify(user));

console.log("user snapshot", snapshot);
```

This works for simple JSON-compatible data.

## Real Pain Points

- Unlabeled logs are hard to understand when there are many of them.
- Sensitive data should not be logged, especially tokens, passwords, or personal
  data.
- Temporary logs should be removed or converted to intentional logging before a
  change is finished.
- `console.time` is useful for rough checks, but serious performance work needs
  better profiling tools.

## Practice

1. Add labeled logs to a function.
2. Print an array of objects with `console.table`.
3. Use `console.time` around a small calculation.
4. Replace a vague log with a useful message and context.

## Related Topics

- [Debugging](debugging.md)
- [DevTools](devtools.md)
- [Errors](errors.md)

