# Spring Boot JWT and CORS Example

This example shows a practical baseline for a stateless Spring Boot REST API
that validates bearer JWTs and allows browser requests from known frontend
origins.

It assumes JWTs are issued by an external identity provider or authorization
server. The API acts as an OAuth2 Resource Server.

## Dependencies

```text
spring-boot-starter-web
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

Enable configuration properties scanning in the application:

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
@EnableWebSecurity
@EnableMethodSecurity
class SecurityConfig {
    @Bean
    SecurityFilterChain securityFilterChain(
        HttpSecurity http,
        CorsConfigurationSource corsConfigurationSource
    ) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .cors(cors -> cors.configurationSource(corsConfigurationSource))
            .sessionManagement(session ->
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            )
            .authorizeHttpRequests(auth -> auth
                .requestMatchers(HttpMethod.OPTIONS, "/**").permitAll()
                .requestMatchers("/actuator/health").permitAll()
                .requestMatchers(HttpMethod.GET, "/api/public/**").permitAll()
                .requestMatchers(HttpMethod.POST, "/api/orders/**")
                    .hasAuthority("SCOPE_orders:write")
                .requestMatchers(HttpMethod.GET, "/api/orders/**")
                    .hasAuthority("SCOPE_orders:read")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 ->
                oauth2.jwt(Customizer.withDefaults())
            );

        return http.build();
    }

    @Bean
    UrlBasedCorsConfigurationSource corsConfigurationSource(
        CorsProperties properties
    ) {
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowedOrigins(properties.allowedOrigins());
        config.setAllowedMethods(List.of("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"));
        config.setAllowedHeaders(List.of("Authorization", "Content-Type", "X-Request-Id"));
        config.setExposedHeaders(List.of("X-Request-Id"));
        config.setAllowCredentials(false);
        config.setMaxAge(3600L);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", config);
        return source;
    }
}
```

## Controller Example

```java
@RestController
@RequestMapping("/api/orders")
class OrderController {
    private final OrderService orderService;

    OrderController(OrderService orderService) {
        this.orderService = orderService;
    }

    @GetMapping("/{id}")
    OrderResponse findById(@PathVariable Long id) {
        return orderService.findById(id);
    }

    @PostMapping
    @PreAuthorize("hasAuthority('SCOPE_orders:write')")
    ResponseEntity<OrderResponse> create(@Valid @RequestBody CreateOrderRequest request) {
        OrderResponse created = orderService.create(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
}
```

## Why This Shape

- `SecurityFilterChain` is explicit and modern.
- OAuth2 Resource Server validates JWT signatures and token metadata.
- The API stays stateless and does not create server sessions.
- CSRF is disabled because bearer tokens are sent explicitly in the
  `Authorization` header, not automatically through cookies.
- CORS allows known frontend origins rather than every origin.
- Authorization uses authorities such as `SCOPE_orders:read`.
- Method security protects sensitive use cases even if routing changes.

## Production Notes

- Validate issuer configuration against the real identity provider.
- Consider audience validation if the identity provider issues tokens for
  multiple APIs.
- Keep allowed origins environment-specific.
- Do not use wildcard origins for private APIs.
- Do not store JWTs in local storage if the frontend can avoid it; token storage
  is a frontend security decision with XSS trade-offs.
- Log authentication failures carefully without logging full tokens.

## Key Idea

For standard JWT-secured APIs, prefer Spring Security OAuth2 Resource Server,
explicit CORS configuration, stateless sessions, and authority-based access
rules over custom token parsing filters.
