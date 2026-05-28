# Code Organization

## Goal

Learn how to organize Java code around responsibilities and boundaries.

## Why It Matters

Good organization helps developers find code quickly and understand what can
depend on what. Poor organization creates circular dependencies, vague packages,
and services that know too much about unrelated details.

## Organize by Feature or Responsibility

For backend code, feature-oriented packages are often easier to navigate than
large technical buckets.

```text
com.example.orders
  api
  service
  domain
  persistence
```

This keeps order-related code close together.

## Keep Domain Concepts Clear

```text
orders/domain/Order.java
orders/domain/OrderStatus.java
orders/service/OrderPaymentService.java
orders/persistence/OrderRepository.java
```

Names should describe business concepts, not only technical mechanisms.

## Boundaries

Separate core logic from external details.

```java
interface OrderRepository {
    java.util.Optional<Order> findById(OrderId id);
    void save(Order order);
}
```

The service can depend on this interface. The database implementation can live
behind it.

## Package-Private Helpers

Do not make every class public.

```java
class OrderStatusParser {
}
```

Package-private helpers keep internal details inside the package.

## Avoid Cycles

Package cycles make code harder to change.

Avoid:

```text
orders -> payments -> orders
```

Use clear boundaries, interfaces, or events when two areas need to communicate.

## Practical Example

```text
com.example.registration
  api/
    RegistrationController.java
  service/
    RegistrationService.java
  domain/
    User.java
    EmailAddress.java
  persistence/
    UserRepository.java
    JdbcUserRepository.java
```

The service owns the use case. The domain package owns core concepts. The
persistence package owns storage details.

## Common Mistakes

- Creating one huge `service` package for everything.
- Putting all helpers in `utils`.
- Making all classes public.
- Letting API classes depend directly on database details.
- Allowing circular dependencies between features.

## Interview Questions

1. Why organize code by feature?
2. What belongs in a domain package?
3. Why use package-private classes?
4. What is a package cycle?
5. How do interfaces help maintain boundaries?

## Practice

1. Reorganize a small app into feature packages.
2. Move a helper class to package-private access.
3. Identify a dependency that crosses a boundary awkwardly.
4. Replace direct infrastructure dependency with an interface.

## Related Topics

- [Packages and Modules](../09_jvm_build_tools/packages_modules.md)
- [SOLID Principles in Practice](solid_principles.md)
- [Interfaces](../02_oop_core_concepts/interfaces.md)

