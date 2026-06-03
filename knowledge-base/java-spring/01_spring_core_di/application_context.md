# Application Context

The application context is the Spring container that holds beans and knows how
to wire them together.

You usually do not interact with it directly in everyday Spring Boot code. It
works behind the scenes when the application starts.

```java
@SpringBootApplication
public class OrdersApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrdersApplication.class, args);
    }
}
```

`SpringApplication.run(...)` creates the application context, scans for beans,
applies configuration, and starts the application.

## What The Context Does

The context:

- creates beans;
- injects dependencies;
- reads configuration;
- manages bean lifecycle;
- applies framework features like validation, transactions, AOP, and web setup.

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

Avoid pulling dependencies manually from the context:

```java
OrderService service = context.getBean(OrderService.class);
```

That is rarely needed in application code. Prefer constructor injection. It
makes dependencies visible and keeps code easier to test.

## Key Idea

The application context is the place where Spring keeps the object graph of your
application.
