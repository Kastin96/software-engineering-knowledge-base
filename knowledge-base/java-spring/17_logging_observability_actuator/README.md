# Logging, Observability, and Actuator

This section covers the operational side of Spring Boot applications: logs,
health checks, metrics, tracing, runtime diagnostics, and actuator security.

The goal is not to install every monitoring tool. The goal is to make a service
debuggable in production without exposing sensitive internals.

## Topics

- 01\. [Logging Basics](logging_basics.md)
- 02\. [Structured Logging and MDC](structured_logging_mdc.md)
- 03\. [Actuator Basics](actuator_basics.md)
- 04\. [Health and Info Endpoints](health_info_endpoints.md)
- 05\. [Metrics with Micrometer](metrics_micrometer.md)
- 06\. [Observability and Tracing](observability_tracing.md)
- 07\. [Runtime Log Levels](runtime_log_levels.md)
- 08\. [Production Configuration](production_configuration.md)
- 09\. [Actuator Security and Exposure](actuator_security_exposure.md)
- 10\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with logging and actuator basics. Then move to health, metrics, tracing,
and runtime diagnostics. Finish with production configuration and exposure
rules, because these decisions affect security and support workflows.

## Mini Goal

By the end of this section, you should be able to:

- configure useful logs without logging sensitive data;
- expose only the actuator endpoints that production support actually needs;
- use health checks correctly for readiness and liveness;
- publish application metrics through Micrometer;
- understand how tracing connects logs, metrics, and requests;
- change log levels temporarily through actuator when troubleshooting;
- explain which actuator endpoints are safe to expose and which are not.

## Interview Readiness

You should be able to answer:

- What is the difference between logs, metrics, and traces?
- Why is `/actuator/health` usually exposed differently from `/actuator/env`?
- How does Micrometer fit into Spring Boot?
- What is structured logging?
- What is the risk of high-cardinality metric tags?
- When would you change log level at runtime?
- How would you secure actuator endpoints in production?
