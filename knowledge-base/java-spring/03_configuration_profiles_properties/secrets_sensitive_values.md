# Secrets and Sensitive Values

Secrets should not be committed to repository configuration files.

Examples of sensitive values:

- database passwords;
- API tokens;
- private keys;
- OAuth client secrets;
- production connection strings;
- internal service credentials.

## Unsafe Example

```yaml
spring:
  datasource:
    password: production-password
```

This creates audit, rotation, and access-control problems.

## Safer Pattern

```yaml
spring:
  datasource:
    password: ${ORDERS_DB_PASSWORD}
```

The actual value should come from the deployment environment, secret manager, or
platform-specific secret injection.

## Logging Risk

Do not log full configuration objects if they may contain secrets.

If a configuration class contains a token or password, avoid generated
`toString()` output in logs.

## Key Idea

Configuration files can describe where a secret is expected. They should not
store the real secret value.
