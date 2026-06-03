# Aggregation Pipelines

MongoDB aggregation pipelines process documents through stages.

They are useful for reporting, grouping, projections, lookups, calculated
fields, and read models that are awkward as simple find queries.

## Example

```java
Aggregation aggregation = Aggregation.newAggregation(
    Aggregation.match(Criteria.where("status").is("PAID")),
    Aggregation.group("customerId")
        .count().as("orderCount")
        .sum("total").as("totalAmount"),
    Aggregation.sort(Sort.Direction.DESC, "totalAmount")
);

AggregationResults<CustomerOrderSummary> results = mongoTemplate.aggregate(
    aggregation,
    "orders",
    CustomerOrderSummary.class
);
```

## Result Projection

```java
public record CustomerOrderSummary(
    String customerId,
    long orderCount,
    BigDecimal totalAmount
) {
}
```

## Design Notes

Aggregation pipelines are production query logic. They should be readable,
tested, and supported by indexes where possible.

Complex aggregation can be powerful, but it can also hide expensive work in the
database.

## Key Idea

Use aggregation when the database should compute a read model, and treat the
pipeline with the same care as SQL query code.
