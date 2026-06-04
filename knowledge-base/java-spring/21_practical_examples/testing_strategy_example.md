# Testing Strategy Example

Design tests for an order service without turning every test into
`@SpringBootTest`.

## Service Behavior

The service should:

- create orders;
- validate customer existence;
- calculate totals;
- persist order data;
- publish an event when payment succeeds.

## Test Mix

Use:

- unit tests for calculation and domain rules;
- `@WebMvcTest` for controller contract;
- `@DataJpaTest` for custom queries;
- Kafka integration test for event flow;
- one full integration test for the most important business path.

## Example Split

```text
OrderTotalCalculatorTest
OrderControllerTest
OrderRepositoryTest
OrderPaidKafkaIntegrationTest
CreateOrderIntegrationTest
```

## What To Avoid

Avoid:

- full context startup for simple pure logic;
- mocking repositories in repository tests;
- asserting framework internals;
- duplicating the same assertions at every level.

## Review Questions

- Which tests are fast?
- Which tests verify framework wiring?
- Which tests need a real database or broker?
- Which behavior is covered only once at the right level?
