# Locking and Concurrency

Concurrent updates can corrupt business state if they are not controlled.

Transactions alone do not automatically prevent every lost update or conflicting
state transition.

## Optimistic Locking

Optimistic locking uses a version column.

```java
@Version
private Long version;
```

If two transactions load the same entity and both try to update it, the second
commit can fail because the version changed.

This is useful when conflicts are possible but not constant.

## Pessimistic Locking

Pessimistic locking asks the database to lock rows.

```java
@Lock(LockModeType.PESSIMISTIC_WRITE)
@Query("select o from OrderEntity o where o.id = :id")
Optional<OrderEntity> findByIdForUpdate(Long id);
```

This can be useful for high-contention updates, but it increases blocking and
deadlock risk.

## Database Constraints

Unique constraints, foreign keys, and check constraints are also concurrency
tools. Do not rely only on application checks for rules the database can enforce
safely.

## Key Idea

Handle concurrency with the right mix of transaction boundaries, locking,
constraints, and retry behavior.
