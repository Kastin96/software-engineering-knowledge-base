# JWT Secured API

Build a secured order API using Spring Security and JWT resource server support.

## Requirements

- public health endpoint;
- authenticated order lookup endpoint;
- admin-only order cancellation endpoint;
- JWT validation through resource server configuration;
- CORS configured for the frontend origin;
- security tests for allowed and denied access.

## Security Shape

```java
@Bean
SecurityFilterChain security(HttpSecurity http) throws Exception {
    return http
        .csrf(AbstractHttpConfigurer::disable)
        .cors(Customizer.withDefaults())
        .authorizeHttpRequests(registry -> registry
            .requestMatchers("/actuator/health").permitAll()
            .requestMatchers(HttpMethod.POST, "/api/orders/*/cancel").hasAuthority("SCOPE_orders:write")
            .requestMatchers("/api/orders/**").hasAuthority("SCOPE_orders:read")
            .anyRequest().authenticated()
        )
        .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))
        .build();
}
```

## CORS

Configure explicit origins, methods, and headers. Do not use wildcard origins
with credentials.

## Tests

Add controller/security tests for:

- unauthenticated request returns `401`;
- missing authority returns `403`;
- valid authority returns success;
- public health endpoint stays public.

## Review Questions

- Which endpoints are public?
- Which authorities are required?
- Where is JWT validation configured?
- How is CORS different from authentication?
