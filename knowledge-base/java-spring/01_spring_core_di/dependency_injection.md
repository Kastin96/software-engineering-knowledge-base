# Dependency Injection

Dependency injection means a class receives the objects it needs instead of
creating them itself.

Without dependency injection:

```java
class OrderService {
    private final OrderRepository orderRepository = new OrderRepository();
}
```

This looks simple, but it tightly couples `OrderService` to one exact
repository implementation. It is harder to test and harder to replace.

With dependency injection:

```java
class OrderService {
    private final OrderRepository orderRepository;

    OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

Now `OrderService` does not create the repository. It only declares that it
needs one.

## Constructor Injection

Constructor injection is usually the best default in Spring.

```java
@Service
class OrderService {
    private final OrderRepository orderRepository;

    OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

Why this is useful:

- required dependencies are obvious;
- fields can be `final`;
- the object cannot be created in an invalid state;
- tests can pass a fake or mocked dependency directly.

## Real Test Benefit

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

The test does not need the whole Spring application to check basic service
logic.

## Avoid Field Injection

Field injection hides dependencies:

```java
@Service
class OrderService {
    @Autowired
    private OrderRepository orderRepository;
}
```

The class looks like it has a no-argument constructor, but it cannot actually
work without Spring filling the field. This makes tests and object creation less
clear.

## Key Idea

Dependency injection keeps classes focused on what they do, not on how their
dependencies are created.
