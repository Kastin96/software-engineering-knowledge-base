# Metrics with Micrometer

Spring Boot uses Micrometer as the metrics facade. Application code records
metrics through Micrometer, and a registry exports them to a backend such as
Prometheus.

## Prometheus Registry

```groovy
implementation "org.springframework.boot:spring-boot-starter-actuator"
runtimeOnly "io.micrometer:micrometer-registry-prometheus"
```

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
```

After that, Prometheus can scrape:

```text
/actuator/prometheus
```

## Custom Counter

```java
@Service
public class OrderMetrics {
    private final MeterRegistry meterRegistry;

    public OrderMetrics(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
    }

    public void recordOrderCreated(String channel) {
        Counter.builder("orders.created")
            .description("Number of created orders")
            .tag("channel", channel)
            .register(meterRegistry)
            .increment();
    }
}
```

The `channel` tag is acceptable if it has a small controlled set of values, such
as `web`, `mobile`, and `partner`.

## Timer Example

```java
Timer.Sample sample = Timer.start(meterRegistry);
try {
    orderProcessor.process(order);
} finally {
    sample.stop(Timer.builder("orders.processing.duration")
        .tag("channel", order.channel())
        .register(meterRegistry));
}
```

## Cardinality Rule

Do not use unbounded values as metric tags:

- user id;
- email;
- order id;
- request id;
- raw URL with path variables.

High-cardinality tags can damage the metrics backend and make dashboards slow or
expensive.

## Key Idea

Metrics are for trends, rates, latency, saturation, and failure signals. They
are not a replacement for logs.
