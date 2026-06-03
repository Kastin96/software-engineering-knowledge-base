# Service-Level Validation

Not all validation belongs in request DTO annotations.

Application services should enforce rules that depend on state, permissions,
workflow, or persistence.

## Example

```java
@Service
class OrderService {
    private final OrderRepository orderRepository;

    OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    OrderResponse cancel(Long orderId, UserId currentUserId) {
        Order order = orderRepository.findById(orderId)
            .orElseThrow(() -> new OrderNotFoundException(orderId));

        if (!order.canBeCancelledBy(currentUserId)) {
            throw new OrderCancellationNotAllowedException(orderId);
        }

        order.cancel();
        orderRepository.save(order);
        return OrderResponse.from(order);
    }
}
```

The request can validate that `orderId` is positive. The service validates
whether the order exists and whether cancellation is allowed.

## Boundary Versus Behavior

Boundary validation:

- "quantity must be positive";
- "email must be present";
- "page size must not exceed 100".

Service-level validation:

- "user can update this resource";
- "order is still cancellable";
- "email is not already registered";
- "account has enough balance".

## Key Idea

Use annotations for structural input constraints. Use services for rules that
depend on application state or behavior.
