# Feature Flags

Feature flags allow runtime configuration to enable or disable behavior.

They are useful for staged rollout, temporary compatibility modes, and guarded
features. They are also a common source of long-term complexity.

## Simple Example

```yaml
app:
  features:
    new-pricing-enabled: false
```

```java
@ConfigurationProperties(prefix = "app.features")
public record FeatureProperties(boolean newPricingEnabled) {
}
```

```java
if (features.newPricingEnabled()) {
    return newPricing.calculate(order);
}

return legacyPricing.calculate(order);
```

## Risks

Feature flags can:

- create multiple active code paths;
- increase test combinations;
- make behavior environment-dependent;
- stay in the codebase after they are no longer needed.

## Operational Discipline

For each flag, know:

- what behavior it controls;
- who owns it;
- how it is tested;
- when it should be removed.

## Key Idea

Feature flags are operational tools, not permanent architecture. Use them
deliberately and remove stale flags.
