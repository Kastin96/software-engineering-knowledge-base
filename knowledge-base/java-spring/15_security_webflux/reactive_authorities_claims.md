# Reactive Authorities and Claims

JWT claims need to become Spring Security authorities before authorization rules
can use them.

For standard OAuth2 scopes, Spring Security commonly maps scopes to authorities
such as:

```text
SCOPE_orders:read
SCOPE_orders:write
```

## Endpoint Rule

```java
.pathMatchers(HttpMethod.POST, "/api/orders/**")
    .hasAuthority("SCOPE_orders:write")
```

## Custom Claim Mapping

Reactive JWT configuration can use a converter from `Jwt` to a reactive
authentication token.

```java
@Bean
Converter<Jwt, Mono<AbstractAuthenticationToken>> jwtAuthenticationConverter() {
    JwtAuthenticationConverter delegate = new JwtAuthenticationConverter();
    delegate.setJwtGrantedAuthoritiesConverter(new JwtGrantedAuthoritiesConverter());
    return new ReactiveJwtAuthenticationConverterAdapter(delegate);
}
```

Then wire it:

```java
.oauth2ResourceServer(oauth2 -> oauth2
    .jwt(jwt -> jwt.jwtAuthenticationConverter(jwtAuthenticationConverter()))
)
```

## Key Idea

Token validation and authority mapping are different steps. WebFlux uses
reactive converter types when customization is needed.
