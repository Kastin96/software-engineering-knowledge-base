# Authorities and Claims

JWT claims need to be mapped into Spring Security authorities before they can be
used for authorization.

## Default Scope Mapping

OAuth2 scopes are commonly mapped to authorities with the `SCOPE_` prefix:

```text
scope: "orders:read orders:write"
```

Becomes:

```text
SCOPE_orders:read
SCOPE_orders:write
```

Then endpoint rules can use:

```java
.requestMatchers(HttpMethod.POST, "/api/orders/**").hasAuthority("SCOPE_orders:write")
```

## Role Claims

Some identity providers put roles in custom claims:

```json
{
  "roles": ["ADMIN", "USER"]
}
```

If the token does not use Spring Security's expected scope format, configure a
converter.

## Converter Example

```java
@Bean
JwtAuthenticationConverter jwtAuthenticationConverter() {
    JwtGrantedAuthoritiesConverter scopes = new JwtGrantedAuthoritiesConverter();
    scopes.setAuthorityPrefix("SCOPE_");

    JwtAuthenticationConverter converter = new JwtAuthenticationConverter();
    converter.setJwtGrantedAuthoritiesConverter(scopes);
    return converter;
}
```

For custom role claims, implement a converter that reads the claim and returns
`GrantedAuthority` values such as `ROLE_ADMIN`.

## Key Idea

JWT validation proves the token is trusted. Authority mapping decides what the
authenticated principal is allowed to do.
