# SecurityFilterChain

`SecurityFilterChain` is the modern component-based way to configure Spring
Security for servlet applications.

It replaces older inheritance-based configuration styles.

## Basic Shape

```java
@Configuration
@EnableWebSecurity
class SecurityConfig {
    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/actuator/health").permitAll()
                .anyRequest().authenticated()
            );

        return http.build();
    }
}
```

## Request Rules

Rules are evaluated in order. Put specific rules before broad rules.

```java
.authorizeHttpRequests(auth -> auth
    .requestMatchers(HttpMethod.GET, "/api/products/**").permitAll()
    .requestMatchers("/api/admin/**").hasRole("ADMIN")
    .anyRequest().authenticated()
)
```

## Multiple Filter Chains

Applications can define more than one filter chain for different path groups.
This is useful, but should be used carefully because it increases configuration
complexity.

## Key Idea

`SecurityFilterChain` is where HTTP security rules are assembled. Keep it
explicit, ordered, and easy to audit.
