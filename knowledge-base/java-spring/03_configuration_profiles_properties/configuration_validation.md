# Configuration Validation

Configuration errors should fail fast at startup.

A missing URL, invalid timeout, or negative retry count should not become a
runtime incident after the application starts accepting traffic.

## Example

```java
@Validated
@ConfigurationProperties(prefix = "app.payment")
public record PaymentProperties(
    @NotNull URI baseUrl,
    @NotNull Duration timeout,
    @Min(0) int retryCount
) {
}
```

If required configuration is missing or invalid, the application should fail to
start.

## Useful Validations

Common validation rules:

- required URL is present;
- timeout is positive;
- retry count is not negative;
- pool size is within a sane range;
- feature flag mode is one of known values.

## Validate Meaning, Not Only Shape

Bean Validation can check basic constraints. More complex checks may need custom
validation in a configuration class or startup check.

For example, a service may require either API key authentication or OAuth
configuration, but not both.

## Key Idea

Invalid configuration is a startup problem. Catch it before the service becomes
partially available.
