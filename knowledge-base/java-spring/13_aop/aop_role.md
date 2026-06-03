# AOP Role

AOP separates cross-cutting behavior from the main application flow.

Cross-cutting behavior is logic that applies to many places but is not the main
business responsibility of those places.

## Examples

Good AOP candidates:

- method timing;
- structured operation logging;
- audit events;
- permission checks around annotated operations;
- retry wrappers for specific integration calls;
- metrics around service methods.

Poor AOP candidates:

- core business rules;
- data transformation specific to one use case;
- complex branching workflow;
- behavior that should be obvious in the service method.

## Why It Helps

Without AOP, the same concern may be repeated across many methods:

```java
long started = System.nanoTime();
try {
    return orderService.findById(id);
} finally {
    metrics.record("order.findById", System.nanoTime() - started);
}
```

An aspect can centralize that concern.

## Trade-Off

AOP makes code cleaner at the call site, but behavior becomes less visible.
Use it when the hidden behavior is predictable, documented, and truly
cross-cutting.

## Key Idea

AOP is best for consistent infrastructure behavior around application methods,
not for hiding use-case logic.
