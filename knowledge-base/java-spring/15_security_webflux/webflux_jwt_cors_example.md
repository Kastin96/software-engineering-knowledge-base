# WebFlux JWT and CORS Example

This example shows a stateless Spring Boot WebFlux API that validates bearer
JWTs and allows browser requests from known frontend origins.

It assumes JWTs are issued by an external identity provider. The API acts as a
reactive OAuth2 Resource Server.

## Dependencies

```text
spring-boot-starter-webflux
spring-boot-starter-security
spring-boot-starter-oauth2-resource-server
spring-security-oauth2-jose
```

## Configuration

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://idp.example.com/issuer

app:
  security:
    cors:
      allowed-origins:
        - http://localhost:3000
        - https://app.example.com
```

## Properties

```java
@ConfigurationProperties(prefix = "app.security.cors")
public record CorsProperties(
    List<String> allowedOrigins
) {
}
```

Enable configuration properties scanning:

```java
@ConfigurationPropertiesScan
@SpringBootApplication
public class OrdersApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrdersApplication.class, args);
    }
}
```

## Security Configuration

```java
@Configuration
@EnableWebFluxSecurity
@EnableReactiveMethodSecurity(useAuthorizationManager = true)
class ReactiveSecurityConfig {
    @Bean
    SecurityWebFilterChain securityWebFilterChain(
        ServerHttpSecurity http,
        org.springframework.web.cors.reactive.CorsConfigurationSource corsConfigurationSource
    ) {
        return http
            .csrf(ServerHttpSecurity.CsrfSpec::disable)
            .cors(cors -> cors.configurationSource(corsConfigurationSource))
            .authorizeExchange(auth -> auth
                .pathMatchers(HttpMethod.OPTIONS, "/**").permitAll()
                .pathMatchers("/actuator/health").permitAll()
                .pathMatchers(HttpMethod.GET, "/api/public/**").permitAll()
                .pathMatchers(HttpMethod.POST, "/api/orders/**")
                    .hasAuthority("SCOPE_orders:write")
                .pathMatchers(HttpMethod.GET, "/api/orders/**")
                    .hasAuthority("SCOPE_orders:read")
                .anyExchange().authenticated()
            )
            .oauth2ResourceServer(oauth2 ->
                oauth2.jwt(Customizer.withDefaults())
            )
            .build();
    }

    @Bean
    org.springframework.web.cors.reactive.UrlBasedCorsConfigurationSource corsConfigurationSource(
        CorsProperties properties
    ) {
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowedOrigins(properties.allowedOrigins());
        config.setAllowedMethods(List.of("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"));
        config.setAllowedHeaders(List.of("Authorization", "Content-Type", "X-Request-Id"));
        config.setExposedHeaders(List.of("X-Request-Id"));
        config.setAllowCredentials(false);
        config.setMaxAge(3600L);

        org.springframework.web.cors.reactive.UrlBasedCorsConfigurationSource source =
            new org.springframework.web.cors.reactive.UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", config);
        return source;
    }
}
```

## Controller Example

```java
@RestController
@RequestMapping("/api/orders")
class ReactiveOrderController {
    private final ReactiveOrderService orderService;

    ReactiveOrderController(ReactiveOrderService orderService) {
        this.orderService = orderService;
    }

    @GetMapping("/{id}")
    Mono<OrderResponse> findById(@PathVariable String id) {
        return orderService.findById(id);
    }

    @PostMapping
    @PreAuthorize("hasAuthority('SCOPE_orders:write')")
    Mono<ResponseEntity<OrderResponse>> create(@Valid @RequestBody Mono<CreateOrderRequest> request) {
        return request
            .flatMap(orderService::create)
            .map(created -> ResponseEntity.status(HttpStatus.CREATED).body(created));
    }
}
```

## Why This Shape

- `SecurityWebFilterChain` is the WebFlux security chain.
- `ServerHttpSecurity` replaces servlet `HttpSecurity`.
- OAuth2 Resource Server validates JWTs without a custom blocking filter.
- The API stays stateless.
- CSRF is disabled for bearer-token requests sent through the `Authorization`
  header.
- CORS is configured through reactive CORS infrastructure.
- Authorization uses authorities such as `SCOPE_orders:read`.
- The controller returns reactive types and does not call `block()`.

## Production Notes

- Keep allowed origins environment-specific.
- Validate issuer configuration against the identity provider.
- Consider audience validation if tokens can target multiple APIs.
- Avoid blocking authority lookups inside reactive security flows.
- Do not log full tokens.
- Test preflight, unauthorized, forbidden, and allowed requests with
  `WebTestClient`.

## Key Idea

For WebFlux APIs, use reactive Spring Security types: `SecurityWebFilterChain`,
`ServerHttpSecurity`, reactive CORS configuration, and OAuth2 Resource Server JWT
support.
