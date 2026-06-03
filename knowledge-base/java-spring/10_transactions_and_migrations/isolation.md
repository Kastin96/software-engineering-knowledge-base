# Isolation

Isolation controls how concurrent transactions see each other's changes.

Higher isolation can reduce concurrency anomalies but may increase locking and
contention.

## Common Isolation Levels

```text
READ_COMMITTED
REPEATABLE_READ
SERIALIZABLE
```

Exact behavior depends on the database.

## Example

```java
@Transactional(isolation = Isolation.READ_COMMITTED)
void updateInventory(Long productId, int quantity) {
    // update stock
}
```

Most applications rely on the database default isolation level. Change it only
when you understand the anomaly being prevented and the cost.

## Common Concurrency Problems

Isolation relates to issues such as:

- dirty reads;
- non-repeatable reads;
- phantom reads;
- lost updates;
- write skew.

Some problems are better handled with locking, version columns, or explicit
constraints instead of globally increasing isolation.

## Key Idea

Isolation is a database consistency control. Tune it for specific concurrency
problems, not as a generic safety switch.
