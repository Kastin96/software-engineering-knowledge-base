# Mocking

## Goal

Understand how to replace real dependencies in tests with controlled fake ones.

## Why It Matters

Some code depends on APIs, databases, timers, files, random values, or the
current time. Mocks help test your logic without relying on unstable external
systems.

## Explanation

A mock is a controlled replacement for a dependency.

Instead of calling a real API:

```javascript
export async function getUserDisplayName(userId, userApi) {
  const user = await userApi.loadUser(userId);
  return `${user.name} <${user.email}>`;
}
```

Use a fake dependency in the test:

```javascript
import { getUserDisplayName } from "./getUserDisplayName.js";

test("formats a loaded user", async () => {
  const userApi = {
    loadUser: async () => ({
      name: "Alex",
      email: "alex@example.com",
    }),
  };

  const result = await getUserDisplayName(1, userApi);

  expect(result).toBe("Alex <alex@example.com>");
});
```

## Dependency Injection

Passing dependencies into a function makes mocking simple.

```javascript
export function createUserService(userRepository) {
  return {
    async findActiveUser(id) {
      const user = await userRepository.findById(id);

      if (!user?.active) {
        return null;
      }

      return user;
    },
  };
}
```

Test with a fake repository:

```javascript
import { createUserService } from "./userService.js";

test("returns active user", async () => {
  const userRepository = {
    findById: async () => ({ id: 1, active: true }),
  };

  const service = createUserService(userRepository);

  await expect(service.findActiveUser(1)).resolves.toEqual({
    id: 1,
    active: true,
  });
});
```

## Mock Functions

Test frameworks can create mock functions that track calls.

```javascript
test("calls logger when user is created", () => {
  const logger = {
    log: vi.fn(),
  };

  createUser({ name: "Alex" }, logger);

  expect(logger.log).toHaveBeenCalledWith("Created user Alex");
});
```

In Jest, the equivalent is usually `jest.fn()`.

## Mocking Time

Code that reads the current time is easier to test when time is passed in.

```javascript
export function isExpired(expiresAt, now = new Date()) {
  return expiresAt.getTime() <= now.getTime();
}
```

```javascript
import { isExpired } from "./isExpired.js";

test("detects expired date", () => {
  const expiresAt = new Date("2026-01-01T00:00:00Z");
  const now = new Date("2026-01-02T00:00:00Z");

  expect(isExpired(expiresAt, now)).toBe(true);
});
```

## Real Pain Points

- Over-mocking can test the mock setup instead of real behavior.
- Mocking implementation details makes tests fragile.
- External calls should usually be mocked in unit tests, but covered with
  integration tests elsewhere.
- Passing dependencies explicitly often creates simpler tests than hidden module
  mocks.

## Practice

1. Mock an API object with one async method.
2. Test that a logger function was called.
3. Refactor a function to accept a dependency as an argument.
4. Test time-based logic by passing a fixed `now` value.

## Related Topics

- [Unit Testing](unit_testing.md)
- [Pure Functions](../09_patterns_and_best_practices/pure_functions.md)
- [Async and Await](../04_async_javascript/async_await.md)

