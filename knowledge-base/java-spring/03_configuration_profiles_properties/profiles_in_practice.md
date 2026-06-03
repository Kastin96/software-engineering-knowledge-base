# Profiles in Practice

Profiles select environment-specific configuration and, in limited cases,
environment-specific beans.

Common profile names:

```text
local
test
dev
staging
prod
```

## Good Use

Use profiles for environment differences:

```yaml
# application-local.yml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/orders
```

```yaml
# application-prod.yml
spring:
  datasource:
    url: ${ORDERS_DB_URL}
```

The service shape stays the same. Only environment details change.

## Use Bean Profiles Carefully

```java
@Bean
@Profile("local")
PaymentClient localPaymentClient() {
    return new FakePaymentClient();
}
```

This can be useful for local development, but it changes the object graph. If
used heavily, profiles make behavior harder to reason about and test.

## Avoid Profile-Driven Architecture

Profiles should not create completely different applications. If `prod`, `dev`,
and `test` all register different service implementations, the application
contract becomes unclear.

## Key Idea

Use profiles to vary environment configuration, not to fragment the application
design.
