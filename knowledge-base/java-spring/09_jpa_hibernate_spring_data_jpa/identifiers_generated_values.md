# Identifiers and Generated Values

Entity identifiers define how JPA tracks persistent objects.

Identifier strategy affects inserts, batching, database portability, and how
entities behave before and after persistence.

## Common Generated ID

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

`IDENTITY` commonly uses database auto-increment behavior. It is simple, but it
can affect batching because the generated ID is needed after insert.

## Sequence Strategy

```java
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "order_seq")
@SequenceGenerator(name = "order_seq", sequenceName = "orders_id_seq")
private Long id;
```

Sequence-based IDs are common in databases such as PostgreSQL and Oracle.

## Natural IDs

Some values look like identifiers but should not necessarily be primary keys:

- email address;
- order number;
- external provider ID;
- account code.

These values can change or come from external systems. Use database constraints
for uniqueness when needed, but choose primary keys deliberately.

## Entity Equality

Entity `equals` and `hashCode` are easy to get wrong when IDs are generated.
Be careful when placing mutable or not-yet-persisted entities in hash-based
collections.

## Key Idea

Identifier strategy is a persistence design decision. It is not just an
annotation detail.
