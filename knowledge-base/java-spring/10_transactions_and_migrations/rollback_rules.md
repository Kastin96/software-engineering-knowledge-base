# Rollback Rules

Spring rolls back transactions for unchecked exceptions by default.

Checked exceptions do not trigger rollback by default unless configured.

## Default Behavior

```java
@Transactional
void capturePayment(Long orderId) {
    // RuntimeException -> rollback
    // checked exception -> usually no rollback by default
}
```

## Explicit Rollback

```java
@Transactional(rollbackFor = PaymentProviderException.class)
void capturePayment(Long orderId) throws PaymentProviderException {
    // rollback for this checked exception
}
```

## Do Not Swallow Exceptions Accidentally

```java
@Transactional
void updateOrder(Long orderId) {
    try {
        orderRepository.update(orderId);
    } catch (DataAccessException ex) {
        log.warn("Update failed", ex);
    }
}
```

If the exception is swallowed, the transaction may still commit. If the failure
should roll back the use case, rethrow an appropriate exception.

## Application Exceptions

Use application exceptions to express meaningful rollback-causing failures:

```java
throw new OrderCannotBeUpdatedException(orderId);
```

## Key Idea

Rollback behavior is part of use-case design. Know which failures should commit,
roll back, or be retried.
