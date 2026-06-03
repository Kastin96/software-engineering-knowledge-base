# SQL and NoSQL Boundaries

SQL and NoSQL databases model data differently.

Spring can abstract some access patterns, but it does not make relational and
document databases interchangeable.

## SQL Strengths

Relational databases are strong for:

- joins;
- transactions;
- constraints;
- normalized data;
- reporting queries;
- mature indexing and query planning.

## NoSQL Document Strengths

Document databases such as MongoDB are strong for:

- aggregate-shaped documents;
- flexible schema evolution;
- reads that naturally fetch a whole document;
- embedding related data that is usually consumed together.

## Design Difference

In SQL, related data may be normalized across tables:

```text
orders
order_items
customers
```

In MongoDB, an order document may embed items:

```json
{
  "id": "order-1",
  "items": [
    { "productId": "p1", "quantity": 2 }
  ]
}
```

## Key Idea

Choose SQL or NoSQL based on data shape, query patterns, consistency needs, and
operational constraints. Do not treat database choice as a Spring annotation
change.
