# Domain Logic and Use Cases

Business logic should be easy to find and test.

Avoid scattering rules across controllers, listeners, entity callbacks, and
utility classes.

## Application Service

```java
@Service
public class PaymentUseCase {
    private final OrderRepository orderRepository;
    private final PaymentGateway paymentGateway;

    @Transactional
    public PaymentResult pay(Long orderId, PaymentRequest request) {
        Order order = orderRepository.getRequired(orderId);

        order.validateCanBePaid();
        PaymentConfirmation confirmation = paymentGateway.charge(order, request);
        order.markPaid(confirmation.transactionId());

        return PaymentResult.from(order);
    }
}
```

The service coordinates the use case. It does not need to know HTTP details.

## Domain Method

```java
public void validateCanBePaid() {
    if (status != OrderStatus.CREATED) {
        throw new OrderCannotBePaidException(id, status);
    }
}
```

Rules that belong to the entity can live on the entity. Rules involving multiple
repositories or external systems usually belong in an application service.

## Testing

Use case tests should be possible without starting the whole Spring context when
the logic is pure enough.

## Key Idea

Put business rules where the next developer will look first. For most Spring
services, that means application services and domain objects, not controllers.
