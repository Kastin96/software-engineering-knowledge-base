# Actuator Security and Exposure

Actuator endpoints can reveal sensitive runtime information. Treat them as an
operational API, not as public application routes.

## Exposure Rule

Usually safe to expose publicly or semi-publicly:

- `health`, often with limited details;
- sometimes `info`, if the content is controlled.

Usually restricted to operators:

- `metrics`;
- `prometheus`;
- `loggers`;
- `mappings`;
- `httpexchanges`;
- `threaddump`.

Usually avoid exposing over public HTTP:

- `env`;
- `configprops`;
- `beans`;
- `heapdump`;
- `shutdown`.

## Security Example

```java
@Configuration
public class ActuatorSecurityConfig {

    @Bean
    @Order(1)
    SecurityFilterChain actuatorSecurity(HttpSecurity http) throws Exception {
        return http
            .securityMatcher(EndpointRequest.toAnyEndpoint())
            .authorizeHttpRequests(registry -> registry
                .requestMatchers(EndpointRequest.to("health", "info")).permitAll()
                .anyRequest().hasRole("OPS")
            )
            .httpBasic(Customizer.withDefaults())
            .build();
    }
}
```

This keeps health and info reachable while requiring an operator role for other
actuator endpoints.

## Separate Management Port

```yaml
management:
  server:
    port: 9090
```

A separate management port can be useful when infrastructure routes actuator
traffic through a private network.

## Key Idea

Do not rely on obscurity. If an actuator endpoint can help you debug production,
it can also help an attacker understand production.
