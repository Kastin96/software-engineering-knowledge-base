# Configuration

Configuration tells Spring how to create objects or read settings that may
change between environments.

There are two common meanings:

- Java configuration with `@Configuration` and `@Bean`;
- external configuration with `application.yml` or `application.properties`.

This topic focuses on Java configuration.

## Java Configuration

Use `@Configuration` when you want to define beans manually.

```java
@Configuration
class NotificationConfig {
    @Bean
    EmailSender emailSender() {
        return new EmailSender("noreply@example.com");
    }
}
```

This is useful when the class is not yours, needs setup, or should not be
annotated directly.

## When `@Bean` Is Better Than `@Component`

Use `@Component` when the class is part of your application and has simple
constructor dependencies.

```java
@Service
class InvoiceService {
}
```

Use `@Bean` when object creation is a decision:

```java
@Bean
Clock clock() {
    return Clock.systemUTC();
}
```

This makes time easier to replace in tests.

## Configuration Should Stay Small

Configuration classes should not become business logic classes. If a method
calculates discounts, validates users, or changes orders, it belongs in a
service, not in a config file.

## Key Idea

Configuration is where you describe how infrastructure objects are created.
Business behavior should stay in services.
