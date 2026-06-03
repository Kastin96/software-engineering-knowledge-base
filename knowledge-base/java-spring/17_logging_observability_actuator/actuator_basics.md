# Actuator Basics

Spring Boot Actuator adds production-ready endpoints for inspecting and
monitoring an application.

Add the dependency:

```groovy
implementation "org.springframework.boot:spring-boot-starter-actuator"
```

By default, actuator endpoints are served under `/actuator`.

## Common Endpoints

Commonly useful endpoints:

- `/actuator/health` - application health;
- `/actuator/info` - application metadata;
- `/actuator/metrics` - available metrics;
- `/actuator/prometheus` - Prometheus scrape endpoint when the registry is
  present;
- `/actuator/loggers` - inspect or change logger levels;
- `/actuator/mappings` - inspect request mappings in non-public environments.

## Exposure

Only health is exposed over HTTP by default.

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
```

Expose only what is needed. Endpoints like `env`, `configprops`, `beans`, and
`mappings` can reveal sensitive or internal details.

## Base Path

```yaml
management:
  endpoints:
    web:
      base-path: /actuator
```

Keeping the standard `/actuator` path is usually fine when endpoints are
secured at the network and application level.

## Key Idea

Actuator is not just a developer convenience. In production, it is part of the
support and monitoring surface of the service.
