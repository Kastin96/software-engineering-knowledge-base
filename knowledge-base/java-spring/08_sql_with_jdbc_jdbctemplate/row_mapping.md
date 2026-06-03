# Row Mapping

Row mapping translates database result rows into Java objects.

With `JdbcTemplate`, a `RowMapper` owns this conversion.

## Record Projection

```java
public record OrderRecord(
    Long id,
    String status,
    BigDecimal total
) {
}
```

```java
private static final RowMapper<OrderRecord> ORDER_ROW_MAPPER = (rs, rowNum) ->
    new OrderRecord(
        rs.getLong("id"),
        rs.getString("status"),
        rs.getBigDecimal("total")
    );
```

## Keep Mapping Near The Query

For simple repositories, a private static mapper is often enough. For larger
repositories or shared projections, a dedicated mapper class can reduce
duplication.

## Column Names

Prefer explicit column lists:

```sql
select id, status, total
from orders
where id = ?
```

Avoid `select *` in application queries. It couples mapping to every database
column and can pull unnecessary data.

## Null Handling

Be careful with primitive getters. For nullable numeric columns, `rs.getLong()`
returns `0` for SQL `NULL` unless you check `wasNull()` or use object mapping
carefully.

## Key Idea

Row mapping is part of the persistence boundary. Keep it explicit, tested, and
aligned with selected columns.
