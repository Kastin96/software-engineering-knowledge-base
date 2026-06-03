# Configuration Model

Spring Boot treats configuration as externalized application state.

The same application artifact should be able to run in local, test, staging, and
production environments with different runtime configuration.

## Common Configuration Targets

Configuration usually controls infrastructure and environment behavior:

- database URLs and pool settings;
- external service endpoints;
- request timeouts;
- retry limits;
- logging levels;
- actuator exposure;
- feature flags;
- security integration settings.

## Configuration Should Not Replace Code

This is configuration:

```yaml
app:
  payment:
    base-url: https://payments.example.com
    timeout: 3s
```

This is usually business logic hiding in configuration:

```yaml
app:
  order:
    premium-user-discount-percent: 17
    vip-user-discount-percent: 23
```

Business rules should normally live in code where they can be tested, reviewed,
and versioned as behavior.

## Stable Artifact, Variable Runtime

A healthy deployment model builds one artifact and changes behavior through
runtime configuration:

```text
same jar + local config -> local runtime
same jar + prod config  -> production runtime
```

This avoids rebuilding the service just to change environment details.

## Key Idea

Configuration should describe how the service connects to its environment. It
should not become a second, poorly tested programming language.
