# What Is Spring Boot

Spring Boot is an opinionated setup layer for Spring applications.

It provides defaults for common application types, especially backend services:
web APIs, data access, messaging, validation, security, testing, and operational
features.

Without Boot, a Spring application usually requires more explicit dependency
selection and infrastructure configuration. With Boot, the project starts from a
working baseline and is customized where needed.

## What Boot Adds

Spring Boot commonly provides:

- dependency starters;
- auto-configuration;
- embedded servlet or reactive web server support;
- externalized configuration;
- profile support;
- executable jars;
- production features through actuator;
- test support for common application slices.

## Minimal Application Entry Point

```java
@SpringBootApplication
public class OrdersApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrdersApplication.class, args);
    }
}
```

`@SpringBootApplication` combines component scanning, auto-configuration, and
Spring configuration registration.

## What Boot Does Not Remove

Spring Boot does not remove the need to understand:

- bean wiring;
- application boundaries;
- configuration ownership;
- transaction behavior;
- persistence trade-offs;
- testing strategy;
- production diagnostics.

Boot reduces boilerplate. It does not make architecture decisions for the
service.

## Key Idea

Spring Boot gives a Spring application a strong operational baseline: sensible
defaults, fast setup, externalized configuration, executable packaging, and
integration with common backend infrastructure.
