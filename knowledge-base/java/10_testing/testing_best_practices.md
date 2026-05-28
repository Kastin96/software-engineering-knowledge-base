# Testing Best Practices

## Goal

Learn practical habits that keep Java tests useful as a project grows.

## Why It Matters

A test suite should support change. If tests are flaky, slow, over-mocked, or
hard to understand, developers start avoiding them or ignoring failures.

## Test Behavior

Tests should describe expected behavior.

```java
@Test
void returnsEmptyListWhenNoUsersAreActive() {
}
```

This is better than naming the test after a private helper or branch.

## Keep Tests Deterministic

Control sources of nondeterminism:

- current time;
- random values;
- thread scheduling;
- network calls;
- file system state;
- database contents;
- test execution order.

Inject a clock or supplier when time matters.

```java
class TokenService {
    private final java.time.Clock clock;

    TokenService(java.time.Clock clock) {
        this.clock = clock;
    }
}
```

## Prefer Real Objects for Simple Data

Use real records, DTOs, and value objects.

```java
User user = new User("alex@example.com", true);
```

Mock dependencies, not simple data.

## Avoid Over-Mocking

Over-mocked tests often break during harmless refactoring.

Prefer verifying final state or meaningful external interaction.

```java
assertEquals("PAID", order.status());
```

Only verify calls when the call is the behavior.

## Keep Tests Fast

Unit tests should run quickly. Slow tests belong in a separate integration or
end-to-end category when possible.

Fast tests are more likely to be run locally and in CI.

## Practical Checklist

Before committing tests, ask:

- Does the test name describe behavior?
- Is the setup easy to understand?
- Does the assertion prove the important result?
- Is the test deterministic?
- Does the test avoid real external systems?
- Would the test survive a harmless refactor?
- Does the failure message point to the problem?

## Common Mistakes

- Tests that pass without meaningful assertions.
- Tests that depend on order or shared mutable state.
- Mocks for everything.
- Ignoring flaky tests.
- Testing implementation details.
- Leaving disabled tests without explanation.

## Interview Questions

1. What makes a test flaky?
2. Why is over-mocking harmful?
3. Why should time be injected in tests?
4. What is the difference between unit and integration tests?
5. How do tests support refactoring?

## Practice

1. Find and remove a meaningless assertion.
2. Replace a mocked value object with a real object.
3. Make a time-dependent test deterministic with `Clock.fixed`.
4. Move a slow external test out of a unit-test suite.

## Related Topics

- [Mocking](mocking.md)
- [Test Cases](test_cases.md)
- [Patterns and Best Practices](../11_patterns_and_best_practices/README.md)

