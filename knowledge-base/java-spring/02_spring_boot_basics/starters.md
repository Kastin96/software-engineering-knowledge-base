# Starters

Starters are curated dependency sets for common Spring Boot use cases.

Instead of manually choosing every Spring, logging, JSON, validation, and test
dependency, a starter brings a compatible baseline.

## Common Starters

```text
spring-boot-starter-web
spring-boot-starter-validation
spring-boot-starter-data-jpa
spring-boot-starter-data-mongodb
spring-boot-starter-security
spring-boot-starter-actuator
spring-boot-starter-test
```

Each starter affects both dependencies and auto-configuration.

## Example Maven Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

This adds the typical stack for blocking Spring MVC web applications, including
embedded Tomcat by default.

## Be Deliberate With Starters

Do not add starters "just in case". A starter may activate auto-configuration,
create beans, expose endpoints, or change runtime behavior.

For example, adding a database starter without clear configuration can cause the
application to fail at startup because Boot tries to configure a data source or
repository infrastructure.

## Key Idea

Starters are not just dependency shortcuts. They are signals to Spring Boot
about the kind of application infrastructure you want.
