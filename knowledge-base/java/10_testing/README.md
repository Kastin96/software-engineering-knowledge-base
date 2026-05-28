# Java Testing

This section explains practical Java testing with JUnit Jupiter, assertions,
test-case design, mocks, and maintainable test structure.

After finishing it, you should be able to write focused unit tests, choose useful
test cases, verify behavior with assertions, use mocks only where they help, and
keep tests readable enough to serve as documentation.

## Topics

- 01\. [Unit Testing](unit_testing.md)
- 02\. [JUnit Jupiter](junit_jupiter.md)
- 03\. [Assertions](assertions.md)
- 04\. [Test Cases](test_cases.md)
- 05\. [Mocking](mocking.md)
- 06\. [Test Structure and Naming](test_structure_naming.md)
- 07\. [Testing Best Practices](testing_best_practices.md)

## Suggested Learning Flow

Start with unit testing and JUnit Jupiter. Then learn assertions and test-case
design. After that, study mocking and test structure. Finish with best practices
so your tests stay useful as the code changes.

## Mini Goal

By the end of this section, write tests for a small Java service that:

- validates user input;
- calculates a result;
- handles an error case;
- depends on one external interface;
- uses a mock for that dependency;
- has clear test names and focused assertions.

## Interview Readiness

You should be able to answer:

- What is a unit test?
- What is the arrange-act-assert pattern?
- What is JUnit Jupiter?
- What makes a good test case?
- When should you use a mock?
- What is the difference between testing state and testing interaction?
- Why should tests be deterministic?

