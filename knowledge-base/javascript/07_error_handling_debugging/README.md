# Error Handling and Debugging

This section explains how to understand failures and debug JavaScript programs.

After finishing it, you should be able to read error messages, throw and catch
errors intentionally, debug code step by step, use `console` effectively, and
inspect browser code with DevTools.

## Topics

- 01\. [Errors](errors.md)
- 02\. [try...catch](try_catch.md)
- 03\. [Debugging](debugging.md)
- 04\. [Console](console.md)
- 05\. [DevTools](devtools.md)

## Suggested Learning Flow

Start with errors so you understand what JavaScript is telling you when
something fails. Then learn `try...catch` to handle expected failures. After
that, study debugging workflow, `console`, and DevTools so you can investigate
real problems without guessing.

## Mini Goal

By the end of this section, try to debug a small script that:

- reads user input;
- validates the input;
- throws a useful error for invalid data;
- catches the error at the boundary;
- logs enough context to understand the failure;
- uses a breakpoint to inspect values step by step.

