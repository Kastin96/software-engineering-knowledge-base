# Architecture Exercises

## Exercise 1: Refactor Controller Logic

Move business logic out of a controller into an application service.

Acceptance criteria:

- controller handles HTTP only;
- use case owns transaction boundary;
- tests become more focused;
- API behavior stays the same.

## Exercise 2: Define Module Boundary

Split order and payment code into clear packages.

Acceptance criteria:

- package structure shows ownership;
- payment does not depend on order internals unnecessarily;
- shared DTOs are not used as a shortcut across boundaries.

## Exercise 3: Choose Integration Style

Decide whether a new workflow should use REST or Kafka.

Acceptance criteria:

- consistency requirement is stated;
- latency requirement is stated;
- failure handling is described;
- trade-off is explained clearly.
