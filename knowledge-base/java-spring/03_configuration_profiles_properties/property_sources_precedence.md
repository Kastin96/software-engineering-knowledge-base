# Property Sources and Precedence

Spring Boot can read the same property from multiple sources.

When the same key exists in more than one place, precedence decides which value
wins. This is useful, but it can also make runtime behavior surprising.

## Common Sources

Typical property sources include:

- default values in code;
- `application.yml`;
- profile-specific files such as `application-prod.yml`;
- environment variables;
- command-line arguments;
- test-specific property overrides;
- deployment platform configuration.

## Example

```yaml
# application.yml
app:
  payment:
    timeout: 3s
```

```yaml
# application-prod.yml
app:
  payment:
    timeout: 1s
```

If the `prod` profile is active, the production value overrides the base value.

## Operational Risk

If a service behaves differently than expected, check:

- active profiles;
- environment variables;
- command-line arguments;
- Kubernetes or platform config;
- test annotations overriding properties;
- default values in `@ConfigurationProperties`.

## Keep Overrides Visible

Prefer a small number of configuration layers. Too many override points make it
hard to explain which value the service is actually using.

## Key Idea

Configuration precedence is powerful, but it should be predictable. Keep the
source of important runtime values easy to trace.
