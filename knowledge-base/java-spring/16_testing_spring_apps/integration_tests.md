# Integration Tests

Integration tests verify that multiple application layers work together.

Use them for important flows where wiring, configuration, transactions,
serialization, security, and persistence need to be tested together.

## Example Shape

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class CreateOrderIntegrationTest {
    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    void createsOrder() {
        CreateOrderRequest request = new CreateOrderRequest("customer-1", List.of(item()));

        ResponseEntity<OrderResponse> response = restTemplate.postForEntity(
            "/api/orders",
            request,
            OrderResponse.class
        );

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        assertThat(response.getBody().status()).isEqualTo("CREATED");
    }
}
```

## Keep Integration Tests Focused

Do not turn every edge case into a full integration test. Cover the critical
paths and use smaller tests for variations.

## Useful Coverage

Integration tests are good for:

- application startup;
- endpoint-to-database flows;
- transaction behavior;
- security wiring;
- serialization and validation together;
- external infrastructure through test doubles or containers.

## Key Idea

Integration tests are expensive but valuable. Use them for confidence across
boundaries, not for every small branch.
