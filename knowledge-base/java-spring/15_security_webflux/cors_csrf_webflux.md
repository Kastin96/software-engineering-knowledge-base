# CORS and CSRF in WebFlux

CORS and CSRF have the same purpose in WebFlux as in servlet applications, but
the configuration types are reactive.

## CORS

For WebFlux, use reactive CORS infrastructure:

```java
@Bean
UrlBasedCorsConfigurationSource corsConfigurationSource(CorsProperties properties) {
    CorsConfiguration config = new CorsConfiguration();
    config.setAllowedOrigins(properties.allowedOrigins());
    config.setAllowedMethods(List.of("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"));
    config.setAllowedHeaders(List.of("Authorization", "Content-Type", "X-Request-Id"));

    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**", config);
    return source;
}
```

In WebFlux, the `UrlBasedCorsConfigurationSource` type comes from
`org.springframework.web.cors.reactive`.

## Security Chain Integration

```java
return http
    .cors(cors -> cors.configurationSource(corsConfigurationSource))
    .build();
```

## CSRF

For stateless bearer-token APIs, CSRF is commonly disabled:

```java
.csrf(ServerHttpSecurity.CsrfSpec::disable)
```

Do not apply this blindly to cookie/session-based browser applications.

## Key Idea

Use reactive CORS types for WebFlux, and make CSRF decisions based on the
authentication mechanism.
