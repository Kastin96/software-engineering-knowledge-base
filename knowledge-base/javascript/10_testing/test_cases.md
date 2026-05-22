# Test Cases

## Goal

Learn how to choose useful test cases for JavaScript code.

## Why It Matters

Good tests are not about testing every possible input. They are about choosing
examples that prove important behavior and protect against likely bugs.

## Start With Behavior

Define what the code should do.

```javascript
export function getDiscountPercent(user) {
  if (user.subscription === "pro") {
    return 20;
  }

  if (user.subscription === "basic") {
    return 10;
  }

  return 0;
}
```

Useful test cases:

- pro user gets 20;
- basic user gets 10;
- unknown subscription gets 0.

```javascript
test("returns 20 for pro users", () => {
  expect(getDiscountPercent({ subscription: "pro" })).toBe(20);
});

test("returns 10 for basic users", () => {
  expect(getDiscountPercent({ subscription: "basic" })).toBe(10);
});

test("returns 0 for unknown subscriptions", () => {
  expect(getDiscountPercent({ subscription: "free" })).toBe(0);
});
```

## Normal Cases

Test the expected common path.

```javascript
export function formatFullName(user) {
  return `${user.firstName} ${user.lastName}`;
}
```

```javascript
test("formats first and last name", () => {
  expect(
    formatFullName({
      firstName: "Alex",
      lastName: "Smith",
    }),
  ).toBe("Alex Smith");
});
```

## Edge Cases

Edge cases are unusual but important inputs.

```javascript
export function calculateAverage(numbers) {
  if (numbers.length === 0) {
    return 0;
  }

  const total = numbers.reduce((sum, number) => sum + number, 0);
  return total / numbers.length;
}
```

```javascript
test("returns 0 for an empty list", () => {
  expect(calculateAverage([])).toBe(0);
});
```

## Error Cases

Test failures when they are part of the expected behavior.

```javascript
export function requireEmail(user) {
  if (!user.email) {
    throw new Error("Email is required");
  }

  return user.email;
}
```

```javascript
test("throws when email is missing", () => {
  expect(() => requireEmail({})).toThrow("Email is required");
});
```

## Boundary Cases

Boundary cases test values at the edge of a rule.

```javascript
export function isAdult(age) {
  return age >= 18;
}
```

```javascript
test("returns false for age 17", () => {
  expect(isAdult(17)).toBe(false);
});

test("returns true for age 18", () => {
  expect(isAdult(18)).toBe(true);
});
```

## Regression Cases

When a bug is fixed, add a test that would fail before the fix.

```javascript
export function normalizeTag(tag) {
  return tag.trim().toLowerCase();
}
```

```javascript
test("trims spaces before lowercasing tag", () => {
  expect(normalizeTag("  JavaScript  ")).toBe("javascript");
});
```

## Real Pain Points

- Tests that only cover the happy path miss most real bugs.
- Too many similar tests create maintenance noise without increasing confidence.
- A test name should explain the behavior, not just repeat the function name.
- Regression tests are valuable because they protect against bugs that already
  happened once.

## Practice

1. Write normal, edge, and error cases for a validation function.
2. Test both sides of a boundary rule.
3. Add a regression test for a bug you imagine.
4. Rename a vague test so it describes behavior clearly.

## Related Topics

- [Unit Testing](unit_testing.md)
- [Errors](../07_error_handling_debugging/errors.md)
- [Clean Code](../09_patterns_and_best_practices/clean_code.md)

