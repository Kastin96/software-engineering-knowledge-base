# Consuming Events

Use `@KafkaListener` for most Spring Boot consumers.

Consumer code should be small. Parse the event, validate the business meaning,
call an application service, and let the configured error handling deal with
failures.

## Consumer Example

```java
@Component
public class OrderPaidListener {
    private final InvoiceService invoiceService;

    public OrderPaidListener(InvoiceService invoiceService) {
        this.invoiceService = invoiceService;
    }

    @KafkaListener(
        topics = "orders.paid.v1",
        groupId = "invoice-service"
    )
    public void handle(OrderPaidEvent event) {
        invoiceService.createInvoice(
            event.eventId(),
            event.orderId(),
            event.customerId(),
            event.amount()
        );
    }
}
```

## Consumer Group

Consumers with the same group id share the work. A record from one partition is
processed by one consumer in that group.

Different services usually use different group ids. That lets each service
receive the same topic independently.

## Keep Listener Logic Thin

Prefer:

```text
listener -> application service -> repository/external client
```

Avoid putting complex business logic directly in the listener. It makes testing
and retry behavior harder.

## Batch Consumption

Batch listeners can improve throughput, but they complicate error handling.
Start with record listeners unless there is a clear throughput requirement.

## Key Idea

A Kafka listener is an input adapter. It should translate a message into a use
case call and keep infrastructure concerns out of the business service.
