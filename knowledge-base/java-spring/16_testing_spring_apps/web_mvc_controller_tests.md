# Web MVC Controller Tests

Controller tests should verify the HTTP contract.

They are useful for status codes, request binding, validation, response bodies,
and error handling.

## Example

```java
@WebMvcTest(OrderController.class)
class OrderControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @MockitoBean
    private OrderService orderService;

    @Test
    void findsOrderById() throws Exception {
        when(orderService.findById(42L))
            .thenReturn(new OrderResponse(42L, "PAID", new BigDecimal("99.90")));

        mockMvc.perform(get("/api/orders/{id}", 42))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.id").value(42))
            .andExpect(jsonPath("$.status").value("PAID"));
    }
}
```

## What To Test

Test:

- request paths and HTTP methods;
- status codes;
- JSON request and response shape;
- validation failures;
- error response format;
- security behavior if the controller is protected.

## What Not To Test Here

Do not test repository queries or complex service logic in a controller slice.
Mock the service and focus on the HTTP boundary.

## Key Idea

Controller tests should prove the API contract, not duplicate service tests.
