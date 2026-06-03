# Design Trade-Offs

Architecture decisions have trade-offs. A senior discussion should make those
trade-offs visible.

## Simplicity vs Flexibility

Simple code is easier to read and ship. Flexible code can support change, but
it can also add unnecessary abstraction.

Add flexibility when change is likely or already painful.

## Synchronous vs Asynchronous

Synchronous calls are easier to reason about but couple availability and
latency.

Asynchronous messaging decouples services but adds delivery, ordering,
idempotency, and support complexity.

## One Module vs Many Modules

One module is easier to start. Many modules can make boundaries explicit, but
they require discipline and build complexity.

Split modules when ownership, deployment, or change frequency justifies it.

## Rich Domain vs Transaction Script

Rich domain models keep behavior near state. Transaction scripts keep workflows
straightforward.

For simple CRUD services, transaction scripts can be enough. For complex
business rules, domain methods help keep rules coherent.

## Key Idea

Good design is contextual. The best answer usually explains why a decision fits
the current system, team, and constraints.
