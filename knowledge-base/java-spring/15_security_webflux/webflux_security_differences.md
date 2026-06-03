# How WebFlux Security Differs

WebFlux Security uses the same security ideas as servlet Spring Security but a
different runtime model.

## Key Type Differences

```text
Servlet MVC              WebFlux
-----------------------------------------------------
HttpSecurity             ServerHttpSecurity
SecurityFilterChain      SecurityWebFilterChain
Filter                   WebFilter
authorizeHttpRequests    authorizeExchange
MockMvc                  WebTestClient
JwtDecoder               ReactiveJwtDecoder
```

## Execution Model

Servlet security runs in the servlet request processing model. WebFlux security
runs in the reactive request processing model.

This matters because blocking work in WebFlux can harm event-loop scalability.
Security code should avoid blocking calls to databases, remote services, or
synchronous SDKs.

## Configuration Style

Servlet:

```java
SecurityFilterChain securityFilterChain(HttpSecurity http)
```

WebFlux:

```java
SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http)
```

## Same Concepts

The concepts remain familiar:

- public versus protected endpoints;
- JWT bearer token validation;
- authorities and scopes;
- CORS;
- CSRF;
- method security;
- authorization tests.

## Key Idea

WebFlux Security is not a different security philosophy. It is Spring Security
adapted to the reactive WebFlux runtime.
