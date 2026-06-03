# Test Profiles and Configuration

Tests need controlled configuration.

Do not let tests accidentally depend on local developer settings or production
configuration.

## Test Profile

```java
@ActiveProfiles("test")
@SpringBootTest
class OrderIntegrationTest {
}
```

```yaml
# application-test.yml
app:
  payment:
    base-url: http://localhost:9999
```

## Property Overrides

For focused overrides, use test-specific property sources:

```java
@SpringBootTest(properties = {
    "app.payment.timeout=1s"
})
class PaymentTimeoutTest {
}
```

## Dynamic Properties

Use `@DynamicPropertySource` when values are known only at test runtime, such as
container ports.

## Keep Secrets Out

Tests should not require real production secrets. Use fake credentials,
containers, local test doubles, or mock servers.

## Key Idea

Test configuration should be explicit, isolated, and safe to run anywhere.
