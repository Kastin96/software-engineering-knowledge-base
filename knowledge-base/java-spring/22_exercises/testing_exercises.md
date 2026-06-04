# Testing Exercises

## Exercise 1: Controller Slice

Test a REST controller with `@WebMvcTest`.

Acceptance criteria:

- success response is verified;
- validation failure is verified;
- service dependency is mocked;
- JSON contract is asserted.

## Exercise 2: Repository Test

Test a repository query with a real database test slice.

Acceptance criteria:

- test data is explicit;
- query behavior is asserted;
- generated SQL or query behavior is understood.

## Exercise 3: Integration Test

Test the main business flow with `@SpringBootTest`.

Acceptance criteria:

- only one or two high-value flows use full context;
- external dependencies are replaced with test-friendly adapters or containers;
- assertions verify business outcome, not framework internals.
