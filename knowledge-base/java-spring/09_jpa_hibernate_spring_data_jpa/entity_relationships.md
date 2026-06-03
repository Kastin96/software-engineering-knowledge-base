# Entity Relationships

JPA relationships map associations between entities.

They are useful, but every relationship adds fetching, cascade, ownership, and
performance considerations.

## Many-To-One

```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "customer_id", nullable = false)
private CustomerEntity customer;
```

Many-to-one is common and often useful. Prefer lazy loading unless eager loading
is intentionally required.

## One-To-Many

```java
@OneToMany(mappedBy = "order", cascade = CascadeType.ALL, orphanRemoval = true)
private List<OrderItemEntity> items = new ArrayList<>();
```

One-to-many collections can become expensive if loaded accidentally.

## Owning Side

The owning side controls the foreign key. In bidirectional relationships, one
side owns the database relationship and the other side mirrors it in memory.

This is a common source of bugs when only one side of the object graph is
updated.

## Cascade Carefully

Cascade is not "save everything magically". It defines which operations should
propagate from one entity to related entities.

Use cascade when lifecycle ownership is real. Do not cascade deletes across
shared entities casually.

## Key Idea

Model relationships based on persistence lifecycle and query behavior, not just
because tables have foreign keys.
