# Dependency Injection

Dependency injection means a class declares its collaborators instead of
constructing them internally.

Without dependency injection, the service controls its own dependency creation:

```java
class OrderService {
    private final OrderRepository orderRepository = new OrderRepository();
}
```

That couples `OrderService` to a concrete repository implementation and makes
replacement in tests or alternate runtime configurations harder.

With dependency injection, the dependency becomes part of the service contract:

```java
class OrderService {
    private final OrderRepository orderRepository;

    OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

The service still depends on `OrderRepository`, but it no longer owns the
creation policy.

## Constructor Injection

Constructor injection is the preferred default for required dependencies.

```java
@Service
class OrderService {
    private final OrderRepository orderRepository;

    OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

Why it is useful:

- required dependencies are obvious;
- fields can be `final`;
- invalid partial construction is avoided;
- unit tests can instantiate the class without starting Spring.

## Testability

```java
class OrderServiceTest {
    @Test
    void calculatesTotal() {
        OrderRepository repository = new InMemoryOrderRepository();
        OrderService service = new OrderService(repository);

        // test service behavior
    }
}
```

For service-level behavior, this is often enough. Use Spring integration tests
when the wiring, configuration, persistence, or web layer is part of what you
need to verify.

## Avoid Field Injection

Field injection hides required collaborators and makes the class harder to use
outside the container:

```java
@Service
class OrderService {
    @Autowired
    private OrderRepository orderRepository;
}
```

It also prevents `final` dependencies and makes tests more dependent on Spring
or reflection-based setup.

## Key Idea

Dependency injection separates object behavior from object assembly. Spring owns
assembly; the class owns behavior.
