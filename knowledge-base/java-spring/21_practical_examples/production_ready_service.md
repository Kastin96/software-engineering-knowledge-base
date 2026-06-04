# Production Ready Service

Take a working Spring Boot service and make it production-ready.

## Requirements

- structured logs;
- actuator health and info endpoints;
- metrics through Micrometer;
- useful HTTP timeouts;
- database pool configuration;
- request validation;
- consistent error responses;
- security for non-public endpoints;
- smoke/integration tests.

## Configuration Checklist

```yaml
spring:
  application:
    name: order-service

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      probes:
        enabled: true
```

## Support Signals

Expose enough signals to answer:

- Is the service up?
- Is it receiving traffic?
- Is latency increasing?
- Are errors increasing?
- Is the database pool exhausted?
- Are downstream calls timing out?
- Did the latest deployment change behavior?

## Tests

Add:

- integration test for the main business flow;
- security test for protected endpoints;
- repository test for important queries;
- controller test for validation and error format.

## Review Questions

- Which actuator endpoints are exposed?
- Which metrics matter for the service?
- What is the rollback signal?
- What should support check first during an incident?
