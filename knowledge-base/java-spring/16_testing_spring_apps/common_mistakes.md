# Common Mistakes

Spring test suites often become slow or brittle because test scope is too broad.

## Using `@SpringBootTest` For Everything

Full context tests are useful, but they are expensive. Use unit tests and slices
when a smaller scope proves the behavior.

## Mocking Persistence Behavior

Mocking a repository does not prove SQL, JPA mapping, MongoDB queries, or
constraints. Use data access tests for persistence behavior.

## Testing Implementation Details

Tests should verify behavior and contracts. Avoid tests that break because a
private helper method changed.

## Shared Mutable Test Data

Tests that depend on execution order or shared database state become flaky.
Keep setup isolated.

## Real External Services In Tests

Do not depend on real third-party systems in normal test runs. Use test doubles,
mock servers, or containers.

## Blocking Reactive Tests

Calling `block()` in every WebFlux test hides reactive behavior. Use
`StepVerifier` and `WebTestClient`.

## Key Idea

Good tests are scoped, deterministic, and tied to behavior that matters.
