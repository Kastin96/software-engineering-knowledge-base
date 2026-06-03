# Inserts, Updates, and Batch Operations

`JdbcTemplate` can execute inserts, updates, deletes, and batch operations.

Write operations should be explicit about affected rows and generated IDs when
the application needs them.

## Update

```java
int updated = jdbcTemplate.update(
    "update orders set status = ? where id = ?",
    status.name(),
    orderId
);

if (updated == 0) {
    throw new OrderNotFoundException(orderId);
}
```

Checking affected rows helps distinguish successful updates from missing
records.

## Insert

```java
KeyHolder keyHolder = new GeneratedKeyHolder();

jdbcTemplate.update(connection -> {
    PreparedStatement ps = connection.prepareStatement(
        "insert into orders (customer_id, status, total) values (?, ?, ?)",
        Statement.RETURN_GENERATED_KEYS
    );
    ps.setLong(1, customerId);
    ps.setString(2, status.name());
    ps.setBigDecimal(3, total);
    return ps;
}, keyHolder);
```

Generated key handling depends on database and schema conventions.

## Batch Update

```java
jdbcTemplate.batchUpdate(
    "insert into order_items (order_id, product_id, quantity) values (?, ?, ?)",
    items,
    100,
    (ps, item) -> {
        ps.setLong(1, orderId);
        ps.setLong(2, item.productId());
        ps.setInt(3, item.quantity());
    }
);
```

Batch operations reduce round trips but should be sized carefully.

## Key Idea

Write operations should check outcomes, handle generated values intentionally,
and participate in a transaction when multiple changes belong together.
