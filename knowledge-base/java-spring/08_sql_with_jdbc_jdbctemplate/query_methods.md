# Query Methods

`JdbcTemplate` provides methods for common query shapes.

The repository should choose the method that matches the expected result: one
value, one row, or many rows.

## Query For Object

```java
int count = jdbcTemplate.queryForObject(
    "select count(*) from orders where customer_id = ?",
    Integer.class,
    customerId
);
```

Use this when exactly one scalar value is expected.

## Query One Row

```java
Optional<OrderRecord> findById(Long id) {
    return jdbcTemplate.query(
        "select id, status, total from orders where id = ?",
        orderRowMapper,
        id
    ).stream().findFirst();
}
```

Using `query(...).stream().findFirst()` avoids `queryForObject` throwing when no
row exists. That can make repository methods easier to express as `Optional`.

## Query Many Rows

```java
List<OrderRecord> findByCustomerId(Long customerId) {
    return jdbcTemplate.query(
        "select id, status, total from orders where customer_id = ? order by id desc",
        orderRowMapper,
        customerId
    );
}
```

## Key Idea

Choose query methods based on cardinality. Make "not found" behavior explicit
instead of accidental.
