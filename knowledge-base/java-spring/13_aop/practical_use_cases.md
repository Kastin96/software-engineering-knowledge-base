# Practical Use Cases

AOP is useful when the same infrastructure behavior must be applied consistently
across many methods.

## Method Timing

Record execution time for selected operations:

```java
@TimedOperation
public OrderResponse findById(Long id) {
    return orderQueryService.findById(id);
}
```

## Auditing

Record user actions around sensitive operations:

```java
@Audited(action = "change-user-role")
public void changeRole(Long userId, Role role) {
}
```

## Authorization Guard

For custom authorization rules, an aspect can enforce checks around annotated
methods. Spring Security method security is often a better default, but custom
aspects can fit organization-specific policies.

## Retry Wrapper

Retries can be cross-cutting for selected integration calls. Be careful with
idempotency and side effects.

## What Not To Put In AOP

Avoid hiding core use-case behavior in aspects. If a developer cannot understand
the service behavior without knowing a private aspect exists, the aspect is
probably doing too much.

## Key Idea

Good aspects are operationally useful and conceptually boring. They should not
make business behavior mysterious.
