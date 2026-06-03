# Testing Aspects

Aspects should be tested because they change runtime behavior outside the normal
method body.

## Unit Test The Aspect Logic

If the advice has meaningful logic, test it directly where practical.

For example, audit payload creation can be tested without starting the whole
Spring context.

## Integration Test Proxy Behavior

Because Spring AOP is proxy-based, at least one test should verify that the
aspect actually applies to a Spring bean.

```java
@SpringBootTest
class AuditAspectIntegrationTest {
    @Autowired
    private OrderService orderService;

    @Test
    void recordsAuditEvent() {
        orderService.cancelOrder(42L);

        // assert audit event was recorded
    }
}
```

## Test Pointcut Scope

If a pointcut is broad, test that it includes intended methods and does not
include unrelated ones.

Annotation-based aspects are usually easier to test because the target is
explicit.

## Key Idea

Test both the aspect logic and whether Spring proxying applies it to the
intended beans.
