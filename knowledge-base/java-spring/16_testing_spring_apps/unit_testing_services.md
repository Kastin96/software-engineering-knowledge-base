# Unit Testing Services

Service logic should often be tested without starting Spring.

If a service can be constructed with plain Java dependencies, the test is fast,
focused, and easier to diagnose.

## Example

```java
class OrderServiceTest {
    private final OrderRepository orderRepository = mock(OrderRepository.class);
    private final PaymentClient paymentClient = mock(PaymentClient.class);
    private final OrderService orderService = new OrderService(orderRepository, paymentClient);

    @Test
    void createsOrderAfterPaymentAuthorization() {
        CreateOrderCommand command = new CreateOrderCommand("customer-1", List.of(item()));

        when(paymentClient.authorize(command)).thenReturn(new PaymentAuthorization("payment-1"));
        when(orderRepository.save(any())).thenAnswer(invocation -> invocation.getArgument(0));

        OrderResponse response = orderService.create(command);

        assertThat(response.status()).isEqualTo("CREATED");
        verify(paymentClient).authorize(command);
        verify(orderRepository).save(any(Order.class));
    }
}
```

## What This Test Proves

This test verifies service orchestration and business decisions. It does not
verify Spring wiring, HTTP mapping, database SQL, or security filters.

## Keep Mocks At Boundaries

Mock dependencies that are outside the unit:

- repositories;
- external clients;
- event publishers;
- clock/time providers.

Avoid mocking value objects or the class being tested.

## Key Idea

If the behavior is plain Java, test it as plain Java. Spring context startup is
not a badge of quality.
