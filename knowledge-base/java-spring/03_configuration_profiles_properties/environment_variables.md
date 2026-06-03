# Environment Variables

Environment variables are a common way to provide deployment-specific
configuration to Spring Boot applications.

They are especially useful for values that should differ per environment:
database URLs, external service endpoints, active profiles, credentials, and
feature toggles.

## Example

```yaml
spring:
  datasource:
    url: ${ORDERS_DB_URL}
    username: ${ORDERS_DB_USERNAME}
    password: ${ORDERS_DB_PASSWORD}
```

At runtime:

```text
ORDERS_DB_URL=jdbc:postgresql://db:5432/orders
ORDERS_DB_USERNAME=orders_app
ORDERS_DB_PASSWORD=...
```

## Defaults

Defaults can be useful for local development:

```yaml
app:
  payment:
    timeout: ${PAYMENT_TIMEOUT:3s}
```

Be careful with defaults for production-sensitive values. A missing production
secret should fail startup, not silently use a local fallback.

## Naming

Use clear, service-specific names:

```text
ORDERS_PAYMENT_BASE_URL
ORDERS_PAYMENT_TIMEOUT
ORDERS_DB_URL
```

This avoids collisions in shared deployment environments.

## Key Idea

Environment variables are a runtime input. Use them for deployment-specific
values, and avoid unsafe defaults for critical production configuration.
