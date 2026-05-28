# Java Patterns and Best Practices

This section explains practical Java design habits and common patterns that make
code easier to read, test, and change.

After finishing it, you should be able to write clearer classes and methods,
prefer immutability where it helps, apply common patterns without overusing them,
recognize common antipatterns, and organize Java code around responsibilities.

## Topics

- 01\. [Clean Code in Java](clean_code_java.md)
- 02\. [Immutability](immutability.md)
- 03\. [SOLID Principles in Practice](solid_principles.md)
- 04\. [Common Java Patterns](common_patterns.md)
- 05\. [Common Antipatterns](common_antipatterns.md)
- 06\. [Code Organization](code_organization.md)
- 07\. [Refactoring Habits](refactoring_habits.md)

## Suggested Learning Flow

Start with clean code and immutability because they improve everyday code
immediately. Then study SOLID principles as practical design heuristics. After
that, learn common patterns and antipatterns. Finish with organization and
refactoring habits.

## Mini Goal

By the end of this section, refactor a small Java service so that it:

- has clear names;
- keeps methods focused;
- avoids hidden global state;
- uses immutable value objects where possible;
- depends on interfaces at boundaries;
- moves repeated branching into a clear strategy or helper;
- has tests that still pass after refactoring.

## Interview Readiness

You should be able to answer:

- What makes Java code readable?
- Why is immutability useful?
- What does dependency inversion mean in practical code?
- When is a design pattern helpful?
- What are common Java antipatterns?
- How do you refactor safely?

