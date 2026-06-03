# Java Spring Learning Path

This section is a practical guide for learning Spring and Spring Boot for
backend Java work.

It focuses on the parts that appear in real services: dependency injection,
configuration, REST APIs, validation, error handling, data access, security,
logging, actuator, testing, production support, and later reactive Spring.

This section does not try to explain every Spring feature at once. The goal is
to understand what a backend developer actually uses, why it exists, and how to
avoid common mistakes.

## Recommended Order

- 01\. [Spring Core and Dependency Injection](01_spring_core_di/README.md)

## Planned Topics

- Spring Core, dependency injection, beans, application context
- Spring Boot basics, starters, auto-configuration, profiles
- REST APIs with Spring MVC
- validation and API error handling
- SQL access with JDBC, JdbcTemplate, JPA, Hibernate, Spring Data JPA
- NoSQL access with Spring Data MongoDB
- transactions and database migrations
- WebFlux basics
- AOP
- Spring Security basics
- testing Spring applications
- logging, actuator, metrics, health checks
- Kafka and messaging basics
- performance and production support
- architecture and practical examples

## How To Use This Section

Read the topics in order if you are new to Spring. If you already know the
basics, use each section README as a checklist and jump to weak areas.

The examples should be small and realistic. A code example is useful only when
it shows how a real Spring application would be written or where a common
mistake usually happens.

## What This Section Should Prepare You For

After finishing this section, you should be able to:

- explain what Spring does in a backend application;
- build a small Spring Boot REST service;
- use dependency injection without turning code into hidden magic;
- validate requests and return clear API errors;
- connect a service to SQL and MongoDB data sources;
- add logging, health checks, and useful tests;
- discuss common Spring topics in interview-friendly language.
