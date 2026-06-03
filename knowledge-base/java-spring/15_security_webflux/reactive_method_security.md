# Reactive Method Security

Method security can also be used in WebFlux applications.

It is useful when service methods need authorization rules independent of the
HTTP route.

## Enable Method Security

```java
@Configuration
@EnableReactiveMethodSecurity(useAuthorizationManager = true)
class MethodSecurityConfig {
}
```

## Example

```java
@Service
class OrderService {
    @PreAuthorize("hasAuthority('SCOPE_orders:write')")
    Mono<OrderResponse> create(CreateOrderCommand command) {
        return orderRepository.save(command.toDocument())
            .map(OrderResponse::from);
    }
}
```

## Reactive Caveat

Keep authorization logic non-blocking. If custom permission checks query a
database or external service, they should use reactive APIs or be carefully
isolated.

## Web Rules And Method Rules

Path-level rules are still useful for broad endpoint protection. Method security
adds use-case-level protection close to the service operation.

## Key Idea

Reactive method security protects service operations, but custom checks should
respect the reactive execution model.
