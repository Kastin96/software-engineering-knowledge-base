# Common Mistakes

Configuration issues often show up as inconsistent behavior between local,
test, and production environments.

## Too Many Override Layers

If values can come from many places, debugging runtime configuration becomes
slow. Keep important configuration paths explicit.

## Hardcoding Production Values

Production URLs, credentials, and environment-specific values should not be
hardcoded in committed files.

## Unsafe Defaults

Defaults are convenient for local development, but dangerous for critical
production settings.

```yaml
spring:
  datasource:
    password: ${ORDERS_DB_PASSWORD:password}
```

This can hide a missing secret. Prefer failing startup for required sensitive
values.

## Scattered `@Value`

Many individual `@Value` fields make configuration ownership unclear.

Use typed configuration classes for grouped settings.

## Business Rules In Properties

Property files should not become the place where business behavior is encoded
without tests.

## Key Idea

Good configuration is explicit, typed, validated, and operationally traceable.
