# Spring Boot Cheatsheet

## Core Ideas

- Spring Boot reduces setup through auto-configuration.
- Starters group common dependencies.
- Configuration belongs in `application.yml`, properties classes, and profiles.
- Beans should express application dependencies clearly.
- Avoid hidden global state and unnecessary static access.

## Common Annotations

```text
@SpringBootApplication
@Configuration
@Bean
@Component
@Service
@Repository
@ConfigurationProperties
@Profile
```

## Good Defaults

- Constructor injection.
- Explicit configuration properties.
- Small focused services.
- Profiles for environment differences.
- Tests that match scope.

## Watch For

- too many beans with unclear ownership;
- business logic in configuration;
- profile-specific behavior that is hard to test;
- relying on auto-configuration without understanding what it created.
