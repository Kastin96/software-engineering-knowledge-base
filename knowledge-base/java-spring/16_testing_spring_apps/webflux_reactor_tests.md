# WebFlux and Reactor Tests

Reactive code should be tested without calling `block()` just to inspect the
result.

Use `StepVerifier` for Reactor service pipelines and `WebTestClient` for
WebFlux HTTP contracts.

## Reactor Service Test

```java
class OrderReactiveServiceTest {
    private final ReactiveOrderRepository orderRepository = mock(ReactiveOrderRepository.class);
    private final OrderReactiveService orderService = new OrderReactiveService(orderRepository);

    @Test
    void returnsOrderResponse() {
        OrderDocument document = new OrderDocument("order-1", "PAID");

        when(orderRepository.findById("order-1")).thenReturn(Mono.just(document));

        StepVerifier.create(orderService.findById("order-1"))
            .expectNextMatches(response ->
                response.id().equals("order-1") && response.status().equals("PAID")
            )
            .verifyComplete();
    }

    @Test
    void returnsErrorWhenOrderIsMissing() {
        when(orderRepository.findById("missing")).thenReturn(Mono.empty());

        StepVerifier.create(orderService.findById("missing"))
            .expectError(OrderNotFoundException.class)
            .verify();
    }
}
```

This tests the reactive sequence behavior: emitted value, completion, and error.

## WebFlux Controller Test

```java
@WebFluxTest(ReactiveOrderController.class)
class ReactiveOrderControllerTest {
    @Autowired
    private WebTestClient webTestClient;

    @MockitoBean
    private ReactiveOrderService orderService;

    @Test
    void findsOrderById() {
        when(orderService.findById("order-1"))
            .thenReturn(Mono.just(new OrderResponse("order-1", "PAID")));

        webTestClient.get()
            .uri("/api/orders/{id}", "order-1")
            .exchange()
            .expectStatus().isOk()
            .expectBody()
            .jsonPath("$.id").isEqualTo("order-1")
            .jsonPath("$.status").isEqualTo("PAID");
    }
}
```

This tests the HTTP contract: route, status, and JSON response.

## Best Practice

Use:

- `StepVerifier` for service methods returning `Mono` or `Flux`;
- `WebTestClient` for WebFlux endpoints;
- no `block()` in tests unless you are intentionally testing a blocking bridge.

## Key Idea

Reactive tests should verify publisher behavior directly instead of converting
everything back to blocking style.
