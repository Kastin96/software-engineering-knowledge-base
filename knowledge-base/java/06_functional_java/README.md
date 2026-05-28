# Functional Java

This section explains the functional-style features commonly used in modern
Java.

After finishing it, you should be able to read and write lambdas, use standard
functional interfaces, apply method references, process collections with streams,
collect results safely, and use `Optional` without turning code into a puzzle.

## Topics

- 01\. [Lambdas](lambdas.md)
- 02\. [Functional Interfaces](functional_interfaces.md)
- 03\. [Method References](method_references.md)
- 04\. [Streams](streams.md)
- 05\. [Stream Collectors](stream_collectors.md)
- 06\. [`Optional`](optional.md)
- 07\. [Functional Style Best Practices](functional_style_best_practices.md)

## Suggested Learning Flow

Start with lambdas and functional interfaces, because streams and method
references build on them. Then learn method references as a readability tool.
After that, study streams, collectors, and `Optional`. Finish with best
practices so you know when functional style helps and when a normal loop is
clearer.

## Mini Goal

By the end of this section, write a small program that:

- filters a list of users;
- maps users to response objects;
- groups orders by status;
- calculates totals with streams;
- uses a method reference where it improves readability;
- returns `Optional` for a missing lookup;
- avoids side effects inside stream pipelines.

## Interview Readiness

You should be able to answer:

- What is a lambda expression?
- What is a functional interface?
- How are `Predicate`, `Function`, `Consumer`, and `Supplier` different?
- What is the difference between `map` and `flatMap`?
- What are intermediate and terminal stream operations?
- When should you use `Optional`?
- Why should stream pipelines avoid side effects?

