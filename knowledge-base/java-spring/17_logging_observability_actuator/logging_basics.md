# Logging Basics

Spring Boot configures logging automatically. In a typical Boot application,
Logback is used through `spring-boot-starter-logging`.

Application code should log business-relevant events, not every method call.
Logs are most useful when they explain what happened, what entity was involved,
and what outcome was produced.

## Service Example

```java
@Service
public class OrderService {
    private static final Logger log = LoggerFactory.getLogger(OrderService.class);

    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public OrderResponse pay(Long orderId) {
        Order order = orderRepository.findById(orderId)
            .orElseThrow(() -> new OrderNotFoundException(orderId));

        order.markPaid();
        orderRepository.save(order);

        log.info("Order payment completed: orderId={}, customerId={}",
            order.getId(),
            order.getCustomerId()
        );

        return OrderResponse.from(order);
    }
}
```

This is useful because it records a completed business event. It does not log
the full entity, request body, card number, token, or internal stack details.

## Log Levels

Use:

- `INFO` for important business and lifecycle events;
- `WARN` for recoverable problems that need attention;
- `ERROR` for failures that need investigation;
- `DEBUG` for temporary diagnostics in lower environments;
- `TRACE` only for narrow troubleshooting.

## Basic Configuration

```yaml
logging:
  level:
    root: INFO
    com.example.orders: INFO
    org.springframework.web: WARN
```

## Key Idea

Good logs are written for future debugging. They should provide context without
leaking sensitive data or drowning production in noise.
