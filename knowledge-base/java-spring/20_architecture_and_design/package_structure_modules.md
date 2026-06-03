# Package Structure and Modules

Package structure should show how the system is organized.

Two common options are layer-first and feature-first.

## Layer-First

```text
com.example.orders
  controller
  service
  repository
  entity
  dto
```

Layer-first is simple and familiar. It works for small services but can become
hard to navigate when features grow.

## Feature-First

```text
com.example.orders
  order
    api
    application
    domain
    persistence
  payment
    api
    application
    domain
    persistence
```

Feature-first keeps related code together and makes module boundaries more
visible.

## Boundary Rule

Packages should not become decorative. If `payment` can freely depend on every
class inside `order`, the boundary is not real.

## Practical Recommendation

For a learning or small production service, feature-first with clear inner
packages is often a good balance:

```text
feature/api
feature/application
feature/domain
feature/persistence
```

## Key Idea

The best package structure is the one that makes ownership and change paths
obvious.
