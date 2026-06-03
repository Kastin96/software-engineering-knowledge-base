# JWT Resource Server

For REST APIs that receive bearer JWTs, Spring Security OAuth2 Resource Server
is usually the right baseline.

The API validates JWTs issued by a trusted authorization server or identity
provider. It should not parse and trust tokens manually in controllers.

## Dependencies

Typical dependencies:

```text
spring-boot-starter-security
spring-boot-starter-oauth2-resource-server
spring-security-oauth2-jose
```

The JOSE support is needed for JWT decoding and verification.

## Issuer Configuration

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://idp.example.com/issuer
```

Spring Security can use the issuer metadata to discover keys and validate
tokens.

## Filter Chain

```java
@Bean
SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests(auth -> auth
            .anyRequest().authenticated()
        )
        .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()));

    return http.build();
}
```

## Key Idea

Use Spring Security's resource server support for JWT validation. Avoid custom
JWT parsing filters unless the use case truly requires them.
