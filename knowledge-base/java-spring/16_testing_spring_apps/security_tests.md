# Security Tests

Security tests should verify access rules, not only successful authenticated
requests.

## What To Test

Test:

- public endpoints are accessible;
- protected endpoints reject unauthenticated requests;
- insufficient authority returns forbidden;
- required authority succeeds;
- method security protects sensitive use cases;
- CORS preflight behavior where relevant.

## MVC Example

```java
@WebMvcTest(OrderController.class)
class OrderControllerSecurityTest {
    @Autowired
    private MockMvc mockMvc;

    @MockitoBean
    private OrderService orderService;

    @Test
    void rejectsUnauthenticatedRequest() throws Exception {
        mockMvc.perform(get("/api/orders/42"))
            .andExpect(status().isUnauthorized());
    }
}
```

## Mock Authentication

Use Spring Security test support to create authenticated requests with specific
roles, authorities, or JWT claims. Do not call a real identity provider in
controller tests.

## Key Idea

Security configuration is behavior. Test denied paths and allowed paths.
