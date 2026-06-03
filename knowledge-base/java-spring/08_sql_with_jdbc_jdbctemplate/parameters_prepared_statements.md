# Parameters and Prepared Statements

SQL parameters should be bound through prepared statements, not concatenated
into SQL strings.

`JdbcTemplate` binds parameters for you when placeholders are used.

## Safe Parameter Binding

```java
List<OrderRecord> findByStatus(String status) {
    return jdbcTemplate.query(
        "select id, status, total from orders where status = ?",
        ORDER_ROW_MAPPER,
        status
    );
}
```

The value is passed separately from the SQL text.

## Unsafe SQL Concatenation

```java
String sql = "select id, status, total from orders where status = '" + status + "'";
```

This creates SQL injection risk and makes query behavior harder to reason about.

## Named Parameters

For queries with many parameters, `NamedParameterJdbcTemplate` can improve
readability.

```java
MapSqlParameterSource params = new MapSqlParameterSource()
    .addValue("customerId", customerId)
    .addValue("status", status);

return namedJdbcTemplate.query(
    """
    select id, status, total
    from orders
    where customer_id = :customerId
      and status = :status
    """,
    params,
    ORDER_ROW_MAPPER
);
```

## Key Idea

Keep SQL text stable and pass values as parameters. This improves safety,
readability, and database plan reuse.
