# Configuration Properties

`@ConfigurationProperties` binds grouped configuration into a typed Java object.

It is usually better than scattering many `@Value` fields across services.

## Example

```yaml
app:
  payment:
    base-url: https://payments.example.com
    timeout: 3s
```

```java
@ConfigurationProperties(prefix = "app.payment")
public record PaymentProperties(
    URI baseUrl,
    Duration timeout
) {
}
```

The service can depend on one typed object:

```java
@Service
class PaymentClient {
    private final PaymentProperties properties;

    PaymentClient(PaymentProperties properties) {
        this.properties = properties;
    }
}
```

## Why This Is Useful

- related settings stay grouped;
- types are explicit;
- configuration can be validated;
- dependencies are easier to see;
- tests can construct properties directly.

## Registering Properties

In Boot applications, configuration properties can be enabled through scanning:

```java
@ConfigurationPropertiesScan
@SpringBootApplication
public class OrdersApplication {
}
```

## Key Idea

Use `@ConfigurationProperties` for grouped application settings. It keeps
configuration explicit, typed, and easier to test.
