# Collection Transformations

## Problem

Transform a list of orders into response records, remove invalid orders, and
calculate totals by status.

## Why This Example Matters

Backend Java code constantly transforms domain data into response data. This
example uses streams where the data flow is clear and a map where lookup/report
shape matters.

## Code

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class OrderReports {
    public List<OrderResponse> paidOrderResponses(List<Order> orders) {
        return orders.stream()
                .filter(order -> order.status().equals("PAID"))
                .filter(order -> order.totalInCents() > 0)
                .map(order -> new OrderResponse(order.id(), order.totalInCents()))
                .toList();
    }

    public Map<String, Integer> totalByStatus(List<Order> orders) {
        return orders.stream()
                .collect(Collectors.groupingBy(
                        Order::status,
                        Collectors.summingInt(Order::totalInCents)
                ));
    }

    public record Order(String id, String status, int totalInCents) {
    }

    public record OrderResponse(String id, int totalInCents) {
    }
}
```

## What It Demonstrates

- `filter` for business conditions;
- `map` for response creation;
- `groupingBy` and `summingInt` for reporting;
- records for immutable values;
- keeping stream pipelines short.

## When a Loop Is Better

Use a loop if the transformation needs complex branching, error collection, or
several side effects. Streams are best when the data flow is linear and readable.

## Practice

1. Add a response field with dollars formatted from cents.
2. Group orders by status and count them.
3. Reject negative totals by throwing `IllegalArgumentException`.
4. Rewrite `paidOrderResponses` using a loop and compare readability.

