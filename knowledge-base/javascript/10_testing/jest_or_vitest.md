# Jest or Vitest

## Goal

Understand the common shape of JavaScript tests written with Jest or Vitest.

## Why It Matters

Jest and Vitest are popular JavaScript testing tools. Their APIs are similar, so
learning the basic structure helps you read and write tests in many projects.

## Test File Structure

Test files often use names like:

```text
math.test.js
math.spec.js
```

Example source file:

```javascript
// math.js
export function multiply(a, b) {
  return a * b;
}
```

Example test file:

```javascript
// math.test.js
import { describe, expect, test } from "vitest";
import { multiply } from "./math.js";

describe("multiply", () => {
  test("multiplies two numbers", () => {
    expect(multiply(3, 4)).toBe(12);
  });
});
```

In Jest, `describe`, `test`, and `expect` are often available globally, depending
on project configuration.

## Common Matchers

Use `toBe` for primitive values.

```javascript
expect(2 + 2).toBe(4);
```

Use `toEqual` for objects and arrays.

```javascript
expect({ name: "Alex" }).toEqual({ name: "Alex" });
```

Use `toContain` for arrays or strings.

```javascript
expect(["js", "ts"]).toContain("js");
```

Use `toThrow` for errors.

```javascript
function fail() {
  throw new Error("Failed");
}

expect(() => fail()).toThrow("Failed");
```

## Async Tests

Use `async` and `await`.

```javascript
test("loads user", async () => {
  const user = await loadUser();

  expect(user.name).toBe("Alex");
});
```

Use `resolves` and `rejects` when they make the intent clearer.

```javascript
await expect(loadUserName()).resolves.toBe("Alex");
await expect(loadMissingUser()).rejects.toThrow("User not found");
```

## Setup and Teardown

Use setup hooks when many tests need the same preparation.

```javascript
import { beforeEach, describe, expect, test } from "vitest";

describe("cart", () => {
  let cart;

  beforeEach(() => {
    cart = createCart();
  });

  test("starts empty", () => {
    expect(cart.items).toEqual([]);
  });
});
```

Keep setup small. Large shared setup makes tests harder to read.

## Running Tests

Example `package.json`:

```json
{
  "scripts": {
    "test": "vitest",
    "test:watch": "vitest --watch"
  },
  "devDependencies": {
    "vitest": "^1.0.0"
  }
}
```

Run:

```bash
npm test
```

## Real Pain Points

- `toBe` checks identity for objects. Use `toEqual` for object content.
- Missing `await` in async tests can make tests pass before the assertion runs.
- Too much shared setup hides what a test actually needs.
- Snapshot tests can become noisy if they cover large unstable output.

## Practice

1. Create a test file for a simple `multiply` function.
2. Use `toEqual` for an object result.
3. Write an async test with `await`.
4. Add a small `beforeEach` setup and keep it readable.

## Related Topics

- [Unit Testing](unit_testing.md)
- [Mocking](mocking.md)
- [Scripts](../06_node_javascript/scripts.md)
