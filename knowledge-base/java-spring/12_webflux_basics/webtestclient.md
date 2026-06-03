# WebTestClient

`WebTestClient` tests WebFlux endpoints with a fluent API.

It can be bound to a running server, an application context, router functions,
or controllers depending on the test style.

## Basic Test Shape

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class OrderControllerTest {
    @Autowired
    private WebTestClient webTestClient;

    @Test
    void findsOrderById() {
        webTestClient.get()
            .uri("/api/orders/{id}", "order-1")
            .exchange()
            .expectStatus().isOk()
            .expectBody()
            .jsonPath("$.id").isEqualTo("order-1");
    }
}
```

## What To Test

Test:

- status codes;
- response body shape;
- validation failures;
- not found behavior;
- error response format;
- streaming behavior when relevant.

## Keep Layers Clear

Use endpoint tests to verify HTTP behavior. Use service tests for business
logic and repository tests for persistence behavior.

## Key Idea

`WebTestClient` is the natural test client for WebFlux HTTP contracts.
