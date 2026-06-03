# Auto-Configuration

Auto-configuration is how Spring Boot creates common infrastructure beans based
on the classpath, existing beans, and configuration properties.

For example, if a web starter is present, Boot can configure web infrastructure.
If a data starter and database driver are present, Boot can configure data
access infrastructure.

## How Boot Decides

Auto-configuration is conditional. Boot checks conditions such as:

- whether a class exists on the classpath;
- whether a bean is already defined;
- whether a property is enabled;
- whether the application is servlet-based or reactive.

The practical effect: adding or removing a dependency can change what Boot
configures.

## Custom Beans Override Defaults

If Boot provides a default but your application defines a more specific bean,
your bean can take precedence.

```java
@Configuration
class ClockConfig {
    @Bean
    Clock clock() {
        return Clock.systemUTC();
    }
}
```

This pattern is common for infrastructure objects that should be explicit in
the application.

## Diagnosing Auto-Configuration

When startup behavior is surprising, inspect:

- dependencies in the build file;
- active profiles;
- relevant `application.yml` properties;
- custom beans that may override Boot defaults;
- the condition evaluation report in debug output.

## Key Idea

Auto-configuration provides defaults when the application context and classpath
indicate a common setup. Understand the conditions before overriding them.
