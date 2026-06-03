# WebFlux Security

This section covers Spring Security for WebFlux applications.

It focuses on what differs from servlet-based Spring Security. The general
security concepts are the same: authentication, authorization, JWT validation,
authorities, CORS, CSRF, method security, and tests. The execution model and
configuration types are different.

## Topics

- 01\. [How WebFlux Security Differs](webflux_security_differences.md)
- 02\. [SecurityWebFilterChain](security_web_filter_chain.md)
- 03\. [Reactive JWT Resource Server](reactive_jwt_resource_server.md)
- 04\. [Reactive Authorities and Claims](reactive_authorities_claims.md)
- 05\. [CORS and CSRF in WebFlux](cors_csrf_webflux.md)
- 06\. [Reactive Method Security](reactive_method_security.md)
- 07\. [Testing WebFlux Security](testing_webflux_security.md)
- 08\. [Common Mistakes](common_mistakes.md)
- 09\. [WebFlux JWT and CORS Example](webflux_jwt_cors_example.md)

## Main Difference From Servlet Security

```text
Servlet stack: HttpSecurity       -> SecurityFilterChain
WebFlux stack: ServerHttpSecurity -> SecurityWebFilterChain
```

Servlet security is built around servlet filters. WebFlux security is built
around reactive `WebFilter` processing and integrates with Reactor context.

## Suggested Learning Flow

Start with the differences from ordinary Spring Security. Then review
`SecurityWebFilterChain`, reactive JWT resource server configuration, authorities,
CORS/CSRF, method security, tests, and the final implementation example.

## Mini Goal

By the end of this section, you should be able to configure a WebFlux API where:

- security uses `SecurityWebFilterChain`;
- JWT validation uses reactive resource server support;
- CORS is configured through reactive CORS infrastructure;
- access rules use `authorizeExchange`;
- method security works with reactive services;
- tests use `WebTestClient`.

## Interview Readiness

You should be able to answer:

- What changes between servlet Spring Security and WebFlux Security?
- What is `SecurityWebFilterChain`?
- What is `ServerHttpSecurity`?
- How does JWT resource server setup look in WebFlux?
- How should CORS be configured for WebFlux security?
- Why should blocking code be avoided in WebFlux security paths?
