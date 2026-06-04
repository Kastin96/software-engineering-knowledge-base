# Security Cheatsheet

## Core Points

- Authentication proves who the caller is.
- Authorization decides what the caller can do.
- CORS is browser access control, not authentication.
- JWT resource server validates tokens issued by an identity provider.

## Spring Security Shape

```java
@Bean
SecurityFilterChain security(HttpSecurity http) throws Exception {
    return http
        .authorizeHttpRequests(registry -> registry
            .requestMatchers("/actuator/health").permitAll()
            .anyRequest().authenticated()
        )
        .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))
        .build();
}
```

## Test

Test:

- unauthenticated -> `401`;
- authenticated but insufficient authority -> `403`;
- valid authority -> success;
- public endpoints remain public.

## Watch For

- wildcard CORS with credentials;
- public actuator endpoints;
- business authorization only in frontend;
- tests that cover only successful requests.
