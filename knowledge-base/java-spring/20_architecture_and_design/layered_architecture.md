# Layered Architecture

A common Spring Boot service uses layers to separate responsibilities.

```text
Controller -> Service -> Repository
```

This structure is not advanced, but it is effective when each layer has a clear
job.

## Controller

Controllers handle HTTP concerns:

- route mapping;
- request validation;
- authentication context;
- response status;
- API DTOs.

```java
@PostMapping
public ResponseEntity<OrderResponse> create(@Valid @RequestBody CreateOrderRequest request) {
    OrderResponse response = orderService.create(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(response);
}
```

## Service

Services coordinate use cases:

- business rules;
- transaction boundary;
- repository calls;
- integration calls;
- domain events.

```java
@Transactional
public OrderResponse create(CreateOrderRequest request) {
    Customer customer = customerRepository.getRequired(request.customerId());
    Order order = Order.create(customer, request.items());
    orderRepository.save(order);
    return OrderResponse.from(order);
}
```

## Repository

Repositories handle persistence:

- queries;
- entity loading;
- projections;
- database-specific access.

## Key Idea

Layers are useful when dependencies point inward and responsibilities stay
clear. They become noise when every layer only forwards the same data.
