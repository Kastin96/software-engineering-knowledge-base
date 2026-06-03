# Testing WebFlux Security

Use `WebTestClient` for WebFlux endpoint security tests.

Security tests should cover unauthenticated, forbidden, and allowed requests.

## Basic Unauthorized Test

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class OrderSecurityTest {
    @Autowired
    private WebTestClient webTestClient;

    @Test
    void rejectsUnauthenticatedRequest() {
        webTestClient.get()
            .uri("/api/orders/42")
            .exchange()
            .expectStatus().isUnauthorized();
    }
}
```

## What To Test

Test:

- public endpoints;
- missing JWT returns `401`;
- insufficient authority returns `403`;
- required scope succeeds;
- CORS preflight for allowed origins;
- error behavior for protected routes.

## Mock JWT

Spring Security test support can attach mock JWT authentication to
`WebTestClient` requests. Use that for controller/security rule tests without a
real identity provider.

## Key Idea

WebFlux security tests should use `WebTestClient` and verify the actual HTTP
contract.
