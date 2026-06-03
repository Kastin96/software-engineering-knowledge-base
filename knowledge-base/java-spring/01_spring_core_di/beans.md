# Beans

A bean is an object managed by the Spring application context.

For a bean, Spring is responsible for creation, dependency resolution, lifecycle
callbacks, and framework integration.

## Component Bean

For application classes, a bean is commonly registered through a stereotype
annotation:

```java
@Service
class PaymentService {
    boolean canCharge(Order order) {
        return order.total().signum() > 0;
    }
}
```

During component scanning, Spring detects the class, creates the bean, and makes
it available for injection.

## Configuration Bean

Use `@Bean` when construction needs to be explicit or the type comes from a
library.

```java
@Configuration
class HttpClientConfig {
    @Bean
    RestClient restClient() {
        return RestClient.create();
    }
}
```

This is common for HTTP clients, mappers, SDK clients, clocks, executors, and
other infrastructure objects.

## Stereotype Annotations

Spring has several common annotations for component beans:

- `@Component` for a generic Spring-managed class;
- `@Service` for business logic;
- `@Repository` for data access;
- `@Controller` or `@RestController` for web endpoints.

They all register beans, but the names communicate intent.

## Common Mistake

Do not make every class a bean. DTOs, request/response records, entities, value
objects, and simple domain objects usually should not be container-managed.

```java
record CreateUserRequest(String email, String name) {
}
```

This is data carried through the application boundary, not a framework-managed
component.

## Key Idea

A bean is an object whose creation and lifecycle are owned by Spring. Use that
ownership for components and infrastructure, not for every object in the codebase.
