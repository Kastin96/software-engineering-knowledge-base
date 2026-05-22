# Testing

This section explains the basics of testing JavaScript code.

After finishing it, you should understand what unit tests are, how to choose
useful test cases, when to use mocks, and how Jest/Vitest-style test files are
usually structured.

## Topics

- 01\. [Unit Testing](unit_testing.md)
- 02\. [Mocking](mocking.md)
- 03\. [Test Cases](test_cases.md)
- 04\. [Jest or Vitest](jest_or_vitest.md)

## Suggested Learning Flow

Start with unit testing so you understand the basic test structure. Then learn
test cases because good tests depend on good examples. After that, study mocking
for code with external dependencies. Finish with Jest/Vitest because those tools
show how this looks in real JavaScript projects.

## Mini Goal

By the end of this section, try to write tests for a small module that:

- validates user input;
- calculates a total;
- handles an error case;
- depends on one external function;
- uses a mock for that external function;
- includes both normal cases and edge cases.

