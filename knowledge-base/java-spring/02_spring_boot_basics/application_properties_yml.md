# Application Properties and YAML

Spring Boot applications usually keep external configuration in
`application.properties` or `application.yml`.

YAML is common for grouped settings because it is easier to scan when
configuration becomes nested.

```yaml
server:
  port: 8080

spring:
  application:
    name: orders-service

app:
  payment:
    timeout: 3s
    base-url: http://localhost:9000
```

## What Belongs In Configuration

Configuration is appropriate for values that differ by environment or should be
changeable without code changes:

- server ports;
- database URLs;
- external service URLs;
- timeouts;
- feature flags;
- logging levels;
- actuator exposure settings.

## Avoid Secrets In Repository Files

Do not commit real passwords, tokens, private keys, or production connection
strings.

Use environment variables, secret managers, deployment configuration, or local
ignored files for sensitive values.

## Property Naming

Prefer grouped, domain-specific configuration:

```yaml
app:
  invoice:
    retry-count: 3
    timeout: 5s
```

This is easier to bind into `@ConfigurationProperties` and easier to document.

## Key Idea

Application configuration should describe environment-specific behavior and
infrastructure settings, not hide business logic in property files.
