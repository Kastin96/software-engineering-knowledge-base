# Read-Only Transactions

Read-only transactions communicate that a method should not modify persistent
state.

```java
@Transactional(readOnly = true)
OrderResponse findById(Long id) {
    return orderRepository.findById(id)
        .map(OrderResponse::from)
        .orElseThrow(() -> new OrderNotFoundException(id));
}
```

## Why Use Them

Read-only transactions can:

- communicate intent;
- avoid accidental write behavior;
- help ORM providers optimize dirty checking in some cases;
- keep lazy-loading behavior predictable during read mapping.

## Not A Security Boundary

`readOnly = true` is not a complete protection against all writes in every
database and driver setup. Treat it as an optimization and intent signal, not as
a security mechanism.

## Class-Level Pattern

```java
@Service
@Transactional(readOnly = true)
class OrderQueryService {
    OrderResponse findById(Long id) {
        // read path
    }
}
```

Write methods can override the default if the same class contains both reads
and writes, though separating command and query services can be cleaner.

## Key Idea

Use read-only transactions to document and optimize read paths, but do not rely
on them as the only write prevention mechanism.
