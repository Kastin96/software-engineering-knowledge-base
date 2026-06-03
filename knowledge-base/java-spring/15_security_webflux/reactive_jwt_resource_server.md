# Reactive JWT Resource Server

WebFlux applications can validate bearer JWTs through Spring Security OAuth2
Resource Server support.

The servlet and reactive setups are conceptually similar, but reactive security
uses `SecurityWebFilterChain` and reactive JWT infrastructure.

## Dependencies

```text
spring-boot-starter-webflux
spring-boot-starter-security
spring-boot-starter-oauth2-resource-server
spring-security-oauth2-jose
```

## Issuer Configuration

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://idp.example.com/issuer
```

## Filter Chain

```java
@Bean
SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http) {
    return http
        .authorizeExchange(auth -> auth
            .anyExchange().authenticated()
        )
        .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))
        .build();
}
```

## Custom Decoder

If custom JWT validation is required, expose or configure a
`ReactiveJwtDecoder`. For many services, `issuer-uri` based auto-configuration
is the cleaner baseline.

## Key Idea

Use reactive resource server support for WebFlux JWT validation. Avoid custom
blocking JWT filters.
