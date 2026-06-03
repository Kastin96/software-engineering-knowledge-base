# Structured Logging and MDC

Plain text logs are readable, but structured logs are easier to search,
aggregate, and correlate in production systems.

Spring Boot supports structured logging formats such as ECS, GELF, and Logstash.
This is useful when logs are consumed by systems like Elasticsearch, OpenSearch,
Grafana Loki, Splunk, or another log platform.

## Structured Logging Configuration

```yaml
logging:
  structured:
    format:
      console: logstash
```

For local development, plain logs may be easier to read. For shared
environments, structured logs usually win because they behave better in log
pipelines.

## Key-Value Logging

With SLF4J 2 style logging, prefer explicit key-value context when it is
available in the project.

```java
log.atInfo()
    .addKeyValue("orderId", order.getId())
    .addKeyValue("customerId", order.getCustomerId())
    .addKeyValue("status", order.getStatus())
    .log("Order status changed");
```

This is better than hiding important values inside a long sentence.

## MDC

MDC stores diagnostic context for the current execution flow. Common values are
request id, user id, tenant id, trace id, and correlation id.

```java
MDC.put("requestId", requestId);
try {
    orderService.pay(orderId);
} finally {
    MDC.remove("requestId");
}
```

Use MDC carefully. In thread pools and async code, context can leak if it is not
cleared or propagated correctly.

## What Not To Log

Do not log:

- passwords;
- access tokens or refresh tokens;
- full authorization headers;
- card numbers or secrets;
- full request bodies by default;
- personally identifiable information unless there is a clear business and
  legal reason.

## Key Idea

Structured logs should make production search precise. MDC and key-value fields
are useful only when they are stable, low-noise, and safe to expose.
