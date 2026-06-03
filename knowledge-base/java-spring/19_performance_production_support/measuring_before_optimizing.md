# Measuring Before Optimizing

Before changing code, collect evidence.

Use logs, metrics, traces, thread dumps, database query plans, and load tests to
understand where time is spent.

## Endpoint Timing

```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    private final OrderService orderService;

    @GetMapping("/{id}")
    public OrderResponse findById(@PathVariable Long id) {
        return orderService.findById(id);
    }
}
```

If this endpoint is slow, do not start inside the controller. Check:

- HTTP request duration metric;
- service method trace/span;
- SQL query duration;
- downstream call duration;
- error and retry count;
- thread pool and connection pool usage.

## Custom Timer

```java
Timer.Sample sample = Timer.start(meterRegistry);
try {
    return orderService.recalculate(orderId);
} finally {
    sample.stop(Timer.builder("orders.recalculate.duration")
        .tag("operation", "recalculate")
        .register(meterRegistry));
}
```

Use custom metrics for important business operations, not every method.

## Load Testing

A useful load test should define:

- request mix;
- expected throughput;
- target latency;
- test duration;
- data volume;
- success criteria.

## Key Idea

Measurement turns performance work from guesswork into engineering.
