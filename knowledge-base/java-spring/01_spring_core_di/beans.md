# Beans

A bean is an object managed by Spring.

When Spring starts, it creates an application context. The context contains the
objects Spring knows how to create and wire. Those objects are beans.

## Component Bean

The most common way to create a bean is to annotate a class:

```java
@Service
class PaymentService {
    boolean canCharge(Order order) {
        return order.total().signum() > 0;
    }
}
```

Spring sees the class during component scan, creates an instance, and can inject
it into other beans.

## Configuration Bean

Use `@Bean` when the object is created by a factory method or comes from a class
you do not control.

```java
@Configuration
class HttpClientConfig {
    @Bean
    RestClient restClient() {
        return RestClient.create();
    }
}
```

This is useful for clients, mappers, SDK objects, and configuration-heavy
objects.

## Stereotype Annotations

Spring has several common annotations for component beans:

- `@Component` for a generic Spring-managed class;
- `@Service` for business logic;
- `@Repository` for data access;
- `@Controller` or `@RestController` for web endpoints.

They all register beans, but the names communicate intent.

## Common Mistake

Do not make every class a Spring bean. Simple value objects, DTOs, records,
entities, and small helper objects often do not need Spring management.

```java
record CreateUserRequest(String email, String name) {
}
```

This is just data. It should not be a bean.

## Key Idea

A bean is not just "a class with an annotation". It is an object Spring owns,
creates, configures, and injects where needed.
