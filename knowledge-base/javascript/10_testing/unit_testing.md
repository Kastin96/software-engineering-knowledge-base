# Unit Testing

## Goal

Understand how to test a small piece of JavaScript code in isolation.

## Why It Matters

Unit tests help you verify behavior quickly. They are useful for functions,
validation rules, calculations, data transformations, and small modules that
should behave the same way every time.

## Explanation

A unit test checks one small unit of behavior.

Example function:

```javascript
export function add(a, b) {
  return a + b;
}
```

Example test:

```javascript
import { add } from "./math.js";

test("adds two numbers", () => {
  expect(add(2, 3)).toBe(5);
});
```

Most JavaScript test tools use the same basic idea:

1. Arrange the input.
2. Act by calling the code.
3. Assert the result.

## Arrange, Act, Assert

```javascript
import { calculateTotal } from "./cart.js";

test("calculates the total price", () => {
  const items = [
    { name: "Keyboard", price: 99 },
    { name: "Mouse", price: 40 },
  ];

  const total = calculateTotal(items);

  expect(total).toBe(139);
});
```

The structure is:

- arrange: create `items`;
- act: call `calculateTotal`;
- assert: check the result.

## Testing Pure Functions

Pure functions are usually the easiest to test.

```javascript
export function normalizeEmail(email) {
  return email.trim().toLowerCase();
}
```

```javascript
import { normalizeEmail } from "./normalizeEmail.js";

test("trims and lowercases an email", () => {
  expect(normalizeEmail("  ALEX@EXAMPLE.COM  ")).toBe("alex@example.com");
});
```

## Testing Error Cases

```javascript
export function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }

  return a / b;
}
```

```javascript
import { divide } from "./divide.js";

test("throws when dividing by zero", () => {
  expect(() => divide(10, 0)).toThrow("Cannot divide by zero");
});
```

## Testing Async Code

```javascript
export async function loadUserName(loadUser) {
  const user = await loadUser();
  return user.name;
}
```

```javascript
import { loadUserName } from "./loadUserName.js";

test("loads a user name", async () => {
  const loadUser = async () => ({ name: "Alex" });

  await expect(loadUserName(loadUser)).resolves.toBe("Alex");
});
```

## Real Pain Points

- Tests that know too much about implementation details break during harmless
  refactors. Prefer testing behavior.
- Tests without clear names are hard to understand when they fail.
- Slow tests are often skipped or ignored. Keep unit tests fast.
- Pure logic is easier to test when side effects are kept at clear boundaries.

## Practice

1. Write a test for a function that adds two numbers.
2. Write a test for a function that normalizes an email.
3. Write a test that expects an error to be thrown.
4. Write an async test for a function that returns a promise.

## Related Topics

- [Pure Functions](../09_patterns_and_best_practices/pure_functions.md)
- [Test Cases](test_cases.md)
- [Jest or Vitest](jest_or_vitest.md)

