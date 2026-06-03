# Configuration

Configuration defines how Spring creates infrastructure objects and how the
application reads environment-specific settings.

There are two common meanings:

- Java configuration with `@Configuration` and `@Bean`;
- external configuration with `application.yml` or `application.properties`.

This topic focuses on Java configuration.

## Java Configuration

Use `@Configuration` when bean creation should be explicit.

```java
@Configuration
class NotificationConfig {
    @Bean
    EmailSender emailSender() {
        return new EmailSender("noreply@example.com");
    }
}
```

This is useful when the type is external, construction needs parameters, or the
object should be selected based on configuration.

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

Configuration describes application wiring and infrastructure setup. Business
behavior should stay in application services or domain code.
