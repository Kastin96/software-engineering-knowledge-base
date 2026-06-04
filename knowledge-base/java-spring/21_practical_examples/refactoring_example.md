# Refactoring Example

Refactor a controller-heavy Spring feature into clearer layers.

## Starting Problem

Symptoms:

- controller contains validation, business rules, repository calls, and mapping;
- response DTOs are built from entities directly;
- transaction boundary is unclear;
- tests require full application startup;
- adding a rule changes too many files.

## Target Structure

```text
OrderController
CreateOrderUseCase
Order
OrderRepository
CreateOrderRequest
OrderResponse
OrderMapper
```

## Refactoring Steps

1. Move business workflow into an application service.
2. Keep HTTP validation on request DTOs.
3. Move entity-to-response mapping out of the controller.
4. Add a transaction boundary to the use case.
5. Add focused tests around the extracted logic.

## Before

```text
controller validates request
controller loads customer
controller calculates total
controller saves entity
controller maps response
```

## After

```text
controller validates request shape
use case executes business flow
repository persists state
mapper returns API response
```

## Review Questions

- Did behavior change?
- Are tests easier to write?
- Is the transaction boundary visible?
- Did the refactor remove coupling or only move code around?
