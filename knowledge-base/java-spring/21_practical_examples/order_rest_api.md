# Order REST API

Build a small REST API for order creation and lookup.

## Requirements

- create an order for a customer;
- validate request fields;
- calculate total amount;
- persist the order in SQL;
- return predictable API errors;
- expose paginated order search by customer id.

## Suggested Structure

```text
order
  api
    OrderController
    CreateOrderRequest
    OrderResponse
  application
    OrderService
  domain
    Order
    OrderItem
  persistence
    OrderEntity
    OrderRepository
```

## Important Decisions

- Keep validation on request DTOs.
- Keep business rules in `OrderService` or domain methods.
- Do not return JPA entities from the controller.
- Use pagination for search endpoints.
- Add a consistent error response with `@RestControllerAdvice`.

## Tests

Add:

- unit test for total calculation;
- `@WebMvcTest` for validation and response contract;
- `@DataJpaTest` for custom queries;
- one integration test for create order flow.

## Review Questions

- Where is the transaction boundary?
- What happens if the customer does not exist?
- How are validation errors represented?
- Which fields are safe to expose in `OrderResponse`?
