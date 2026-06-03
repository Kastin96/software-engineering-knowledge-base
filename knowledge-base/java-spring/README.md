# Java Spring Learning Path

This section is a practical Spring and Spring Boot reference for backend Java
developers.

It assumes basic Java knowledge and a general understanding of what frameworks
do. The focus is on concepts and patterns that appear in production services:
dependency injection, configuration, REST APIs, validation, error handling, data
access, security, logging, actuator, testing, production support, and later
reactive Spring.

The goal is not to list every Spring annotation. The goal is to understand how
Spring applications are structured, where framework boundaries are useful, and
which decisions matter when building maintainable services.

## Recommended Order

- 01\. [Spring Core and Dependency Injection](01_spring_core_di/README.md)
- 02\. [Spring Boot Basics](02_spring_boot_basics/README.md)
- 03\. [Configuration, Profiles, and Properties](03_configuration_profiles_properties/README.md)
- 04\. [Spring Web MVC and REST](04_spring_web_mvc_rest/README.md)
- 05\. [Validation](05_validation/README.md)
- 06\. [Error Handling](06_error_handling/README.md)
- 07\. [Data Access Overview](07_data_access_overview/README.md)
- 08\. [SQL with JDBC and JdbcTemplate](08_sql_with_jdbc_jdbctemplate/README.md)
- 09\. [JPA, Hibernate, and Spring Data JPA](09_jpa_hibernate_spring_data_jpa/README.md)
- 10\. [Transactions and Migrations](10_transactions_and_migrations/README.md)
- 11\. [NoSQL with Spring Data MongoDB](11_nosql_spring_data_mongodb/README.md)
- 12\. [WebFlux Basics](12_webflux_basics/README.md)

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

Read the topics in order if you are building Spring fundamentals. If you already
have some experience, use each section README as a checklist and jump to weak
areas.

Examples should be small, realistic, and tied to a concrete backend concern:
wiring application layers, configuring infrastructure, handling HTTP requests,
testing behavior, or diagnosing production issues.

## What This Section Should Prepare You For

After finishing this section, you should be able to:

- explain what Spring does in a backend application;
- build a small Spring Boot REST service;
- use dependency injection with clear boundaries;
- validate requests and return clear API errors;
- connect a service to SQL and MongoDB data sources;
- add logging, health checks, and useful tests;
- discuss common Spring topics in interview-friendly language.
