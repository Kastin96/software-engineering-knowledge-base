# Application Context

The application context is the Spring container that holds beans and knows how
to wire them together.

In Spring Boot applications, you usually do not access the context directly.
It is created during application startup.

```java
@SpringBootApplication
public class OrdersApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrdersApplication.class, args);
    }
}
```

`SpringApplication.run(...)` creates the context, loads configuration, registers
beans, resolves dependencies, and starts the configured application runtime.

## What The Context Does

The context:

- creates beans;
- injects dependencies;
- reads configuration;
- manages bean lifecycle;
- integrates features such as validation, transactions, AOP, web endpoints, and
  persistence infrastructure.

## Real Example

If Spring finds these classes:

```java
@RestController
class OrderController {
    OrderController(OrderService orderService) {
    }
}

@Service
class OrderService {
    OrderService(OrderRepository orderRepository) {
    }
}

@Repository
class OrderRepository {
}
```

The context understands the dependency chain:

```text
OrderController needs OrderService
OrderService needs OrderRepository
```

Then it creates the objects in the right order.

## Common Mistake

Avoid using the context as a service locator in regular application code:

```java
OrderService service = context.getBean(OrderService.class);
```

That hides dependencies and makes code harder to reason about. Constructor
injection keeps required collaborators visible at the class boundary.

## Key Idea

The application context owns the runtime object graph. Application classes
should declare dependencies, not query the container for them.
