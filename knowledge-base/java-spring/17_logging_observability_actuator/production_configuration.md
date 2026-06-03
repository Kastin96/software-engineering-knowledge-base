# Production Configuration

Production observability should be explicit. The configuration should make clear
what is exposed, what is secured, and where telemetry goes.

## Example Configuration

```yaml
spring:
  application:
    name: order-service

logging:
  level:
    root: INFO
    com.example.orders: INFO
  structured:
    format:
      console: logstash

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus,loggers
  endpoint:
    health:
      probes:
        enabled: true
      show-details: when_authorized
  metrics:
    tags:
      application: ${spring.application.name}
  opentelemetry:
    resource-attributes:
      service.name: ${spring.application.name}
      deployment.environment: production
```

This configuration is a starting point, not a universal default. Some teams do
not expose `loggers` over HTTP and use platform-level controls instead.

## Production Checklist

Check:

- logs are structured in shared environments;
- sensitive fields are not logged;
- health probes are enabled and fast;
- only required actuator endpoints are exposed;
- actuator endpoints are protected by network and application security;
- metrics include stable low-cardinality tags;
- dashboards cover traffic, errors, latency, and saturation;
- alerts are based on user impact, not only CPU or memory;
- runtime debug logging has an ownership process.

## Key Idea

Production support is not only about libraries. It is about predictable signals,
safe access, and clear operational habits.
