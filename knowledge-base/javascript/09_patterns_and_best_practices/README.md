# Patterns and Best Practices

This section explains practical JavaScript patterns and habits that make code
easier to read, test, and change.

After finishing it, you should be able to write clearer functions, avoid
unnecessary mutation, recognize pure functions, use simple functional and
object-oriented patterns, and identify common antipatterns before they become
hard-to-maintain code.

## Topics

- 01\. [Clean Code](clean_code.md)
- 02\. [Immutability](immutability.md)
- 03\. [Pure Functions](pure_functions.md)
- 04\. [Functional Patterns](functional_patterns.md)
- 05\. [Object-Oriented Patterns](object_oriented_patterns.md)
- 06\. [Common Antipatterns](common_antipatterns.md)

## Suggested Learning Flow

Start with clean code because naming, function size, and structure affect every
other topic. Then study immutability and pure functions because they reduce
surprising behavior. After that, learn functional and object-oriented patterns
as tools, not rules. Finish with common antipatterns so you can notice risky
code earlier.

## Mini Goal

By the end of this section, try to refactor a small script so that it:

- uses clear names;
- splits one large function into smaller functions;
- updates data without mutating the original object;
- moves calculations into pure functions;
- avoids hidden global state;
- has one clear boundary for side effects.

