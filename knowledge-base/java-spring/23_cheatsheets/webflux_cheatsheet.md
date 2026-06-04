# WebFlux Cheatsheet

## Types

- `Mono<T>`: zero or one value.
- `Flux<T>`: zero to many values.

## Rules

- Do not call `block()` inside reactive flows.
- Keep blocking repositories out of reactive pipelines.
- Use `StepVerifier` for service tests.
- Use `WebTestClient` for endpoint tests.
- Prefer explicit error handling with `switchIfEmpty`, `onErrorMap`, or
  `onErrorResume` where appropriate.

## Common Operators

```text
map
flatMap
filter
switchIfEmpty
zipWith
collectList
onErrorResume
```

## Quick Questions

- Is every dependency reactive?
- Where does the error become an HTTP response?
- Is ordering important?
- Is backpressure relevant for this flow?
