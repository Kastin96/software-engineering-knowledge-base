# Profiles

Profiles let a Spring Boot application load different configuration for
different environments or runtime modes.

Common profiles:

```text
local
test
dev
staging
prod
```

## Profile-Specific Files

```text
application.yml
application-local.yml
application-prod.yml
```

Base configuration goes into `application.yml`. Environment-specific overrides
go into profile files.

```yaml
# application-local.yml
app:
  payment:
    base-url: http://localhost:9000
```

```yaml
# application-prod.yml
app:
  payment:
    base-url: https://payments.example.com
```

## Activating A Profile

Profiles are usually activated through deployment configuration or environment
variables:

```text
SPRING_PROFILES_ACTIVE=prod
```

Avoid hardcoding production profile activation inside committed application
configuration.

## Bean-Level Profiles

Profiles can also control bean registration:

```java
@Bean
@Profile("local")
PaymentClient fakePaymentClient() {
    return new FakePaymentClient();
}
```

Use this carefully. Too many profile-specific beans can make runtime behavior
hard to reason about.

## Key Idea

Profiles are for environment-specific configuration. Keep the application design
stable across profiles whenever possible.
