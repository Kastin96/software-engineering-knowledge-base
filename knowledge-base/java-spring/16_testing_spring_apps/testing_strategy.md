# Testing Strategy

A good Spring test suite uses different test scopes deliberately.

The mistake is not using `@SpringBootTest`. The mistake is using it for
everything, including tests that only need one service method or one controller
slice.

## Practical Test Layers

```text
unit test              -> service/domain logic without Spring
slice test             -> focused Spring layer, such as MVC or JPA
integration test       -> multiple real layers in one Spring context
end-to-end test        -> deployed app or near-production environment
```

## Scope Rule

Use the smallest scope that proves the behavior.

Examples:

- pure service calculation: unit test;
- controller status/body/validation: web slice;
- JPA query mapping: data slice;
- security rule: security-aware web test;
- controller-to-database happy path: integration test.

## What To Optimize For

Tests should be:

- fast enough to run often;
- deterministic;
- explicit about setup;
- focused on behavior;
- realistic where infrastructure matters;
- easy to diagnose when they fail.

## Key Idea

Spring testing is not one annotation. Pick the test scope based on the behavior
under test.
