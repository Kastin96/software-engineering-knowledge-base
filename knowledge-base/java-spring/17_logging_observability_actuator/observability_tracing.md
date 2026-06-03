# Observability and Tracing

Observability connects logs, metrics, and traces so that production behavior can
be understood across services.

Spring Boot uses Micrometer Observation for metrics and traces. OpenTelemetry
can be used to export telemetry to an observability backend.

## Concepts

- logs describe events;
- metrics describe numeric behavior over time;
- traces show request flow across services;
- spans represent individual operations inside a trace.

## Manual Observation

```java
@Service
public class OrderProcessor {
    private final ObservationRegistry observationRegistry;

    public OrderProcessor(ObservationRegistry observationRegistry) {
        this.observationRegistry = observationRegistry;
    }

    public void process(Order order) {
        Observation.createNotStarted("order.processing", observationRegistry)
            .lowCardinalityKeyValue("channel", order.channel())
            .observe(() -> processInternal(order));
    }

    private void processInternal(Order order) {
        // business operation
    }
}
```

Use low-cardinality tags for observations for the same reason as metrics.

## Annotation-Based Observability

```java
@Observed(
    name = "order.payment",
    contextualName = "pay order",
    lowCardinalityKeyValues = {"component", "orders"}
)
public OrderResponse pay(Long orderId) {
    return paymentService.pay(orderId);
}
```

Annotation scanning requires enabling observation annotations and adding AOP
support.

```yaml
management:
  observations:
    annotations:
      enabled: true
```

```groovy
implementation "org.springframework.boot:spring-boot-starter-aop"
```

## Trace Correlation In Logs

When tracing is configured, trace and span identifiers can be included in logs.
This lets support engineers move from one error log to the related distributed
trace.

## Key Idea

Tracing is useful when a request crosses service boundaries. For a small
single-service application, start with logs and metrics, then add tracing when
the operational need is clear.
