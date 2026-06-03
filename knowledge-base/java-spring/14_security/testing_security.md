# Testing Security

Security tests should verify both authentication and authorization behavior.

Do not test only the happy path.

## What To Test

Test:

- unauthenticated request returns `401`;
- authenticated request with insufficient authority returns `403`;
- authenticated request with required authority succeeds;
- public endpoints are accessible;
- CORS preflight works for allowed origins;
- disallowed origins are not accepted;
- method security protects sensitive service methods.

## Mock MVC Example

```java
@WebMvcTest(OrderController.class)
class OrderControllerSecurityTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    void rejectsUnauthenticatedRequest() throws Exception {
        mockMvc.perform(get("/api/orders/42"))
            .andExpect(status().isUnauthorized());
    }
}
```

## JWT Test Support

Spring Security test support can create mock JWT authentication for controller
tests. Use it to verify authorization rules without calling a real identity
provider.

## Key Idea

Security configuration is application behavior. Test denied and allowed paths,
not only successful authenticated requests.
