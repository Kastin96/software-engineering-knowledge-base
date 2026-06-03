# Propagation

Transaction propagation defines how a method behaves when a transaction already
exists.

The default propagation is usually `REQUIRED`.

## REQUIRED

```java
@Transactional
void updateOrder() {
}
```

`REQUIRED` joins an existing transaction or starts a new one if none exists.
This is the common default for service use cases.

## REQUIRES_NEW

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
void writeAuditEvent() {
}
```

`REQUIRES_NEW` suspends the current transaction and starts a separate one.

This can be useful for audit writes, but it changes consistency behavior. The
audit record may commit even if the outer transaction rolls back.

## SUPPORTS And MANDATORY

Some propagation modes are useful for specialized cases:

- `SUPPORTS` runs with a transaction if one exists;
- `MANDATORY` requires an existing transaction;
- `NOT_SUPPORTED` runs without a transaction.

Do not change propagation casually. It is easy to create surprising behavior.

## Key Idea

Propagation defines transaction composition. Use the default unless the use case
clearly needs different behavior.
