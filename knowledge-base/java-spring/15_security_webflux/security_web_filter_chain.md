# SecurityWebFilterChain

`SecurityWebFilterChain` is the WebFlux equivalent of servlet
`SecurityFilterChain`.

It is configured with `ServerHttpSecurity`.

## Basic Shape

```java
@Configuration
@EnableWebFluxSecurity
class SecurityConfig {
    @Bean
    SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http) {
        return http
            .authorizeExchange(auth -> auth
                .pathMatchers("/actuator/health").permitAll()
                .anyExchange().authenticated()
            )
            .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))
            .build();
    }
}
```

## Path Rules

Use `pathMatchers` in WebFlux security:

```java
.authorizeExchange(auth -> auth
    .pathMatchers(HttpMethod.GET, "/api/public/**").permitAll()
    .pathMatchers(HttpMethod.POST, "/api/orders/**").hasAuthority("SCOPE_orders:write")
    .pathMatchers(HttpMethod.GET, "/api/orders/**").hasAuthority("SCOPE_orders:read")
    .anyExchange().authenticated()
)
```

## Rule Order

Place specific rules before broad rules. Once a broad rule matches, later rules
may never be evaluated for that exchange.

## Key Idea

Use `SecurityWebFilterChain` and `ServerHttpSecurity` for WebFlux. Do not copy
servlet `HttpSecurity` configuration into a reactive application.
