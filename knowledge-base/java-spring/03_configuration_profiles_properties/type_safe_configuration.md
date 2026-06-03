# Type-Safe Configuration

Type-safe configuration groups related settings into a Java type.

For production services, this is usually better than scattering `@Value`
annotations through controllers, services, and configuration classes.

## Example

```yaml
app:
  payment:
    base-url: https://payments.example.com
    timeout: 3s
    retry-count: 2
```

```java
@ConfigurationProperties(prefix = "app.payment")
public record PaymentProperties(
    URI baseUrl,
    Duration timeout,
    int retryCount
) {
}
```

The dependency is explicit:

```java
@Service
class PaymentClient {
    private final PaymentProperties properties;

    PaymentClient(PaymentProperties properties) {
        this.properties = properties;
    }
}
```

## Why It Matters

Typed configuration improves:

- readability;
- refactoring;
- test setup;
- validation;
- ownership of related settings.

## When `@Value` Is Still Fine

`@Value` is acceptable for one-off values in configuration classes. It becomes
hard to maintain when many settings are spread across application services.

## Key Idea

Use `@ConfigurationProperties` when settings belong together and are part of a
real application contract.
