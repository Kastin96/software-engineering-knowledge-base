# Java Exceptions and Debugging

This section explains how Java represents failures and how to debug them in a
calm, systematic way.

After finishing it, you should be able to read stack traces, choose between
checked and unchecked exceptions, use `try`, `catch`, `finally`, and
try-with-resources, create useful custom exceptions, and debug common Java
problems without guessing.

## Topics

- 01\. [Exceptions](exceptions.md)
- 02\. [Checked and Unchecked Exceptions](checked_unchecked.md)
- 03\. [`try`, `catch`, and `finally`](try_catch_finally.md)
- 04\. [Try-With-Resources](try_with_resources.md)
- 05\. [Custom Exceptions](custom_exceptions.md)
- 06\. [Stack Traces](stack_traces.md)
- 07\. [Debugging](debugging.md)

## Suggested Learning Flow

Start with the exception model and the difference between checked and unchecked
exceptions. Then learn `try`, `catch`, `finally`, and try-with-resources. After
that, study custom exceptions and stack traces. Finish with debugging workflow,
because the goal is not just to catch errors but to understand and fix them.

## Mini Goal

By the end of this section, write a small program that:

- validates input and throws a clear exception for invalid data;
- reads a file using try-with-resources;
- catches errors only at a useful boundary;
- keeps the original cause when wrapping an exception;
- prints or logs enough context to diagnose a failure;
- uses a debugger or stack trace to find the failing line.

## Interview Readiness

You should be able to answer:

- What is the difference between checked and unchecked exceptions?
- When should an exception be caught, and when should it propagate?
- Why is swallowing exceptions dangerous?
- What problem does try-with-resources solve?
- Why should custom exceptions keep the original cause?
- How do you read a stack trace from top to bottom?

