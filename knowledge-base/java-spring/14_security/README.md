# Security

This section covers Spring Security basics for backend services.

Spring Security is not only a login form library. In REST APIs, it usually
protects endpoints, validates bearer tokens, maps authenticated principals to
authorities, enforces authorization rules, integrates CORS/CSRF decisions, and
provides a consistent security filter chain.

## Topics

- 01\. [Security Role](security_role.md)
- 02\. [Authentication and Authorization](authentication_authorization.md)
- 03\. [SecurityFilterChain](security_filter_chain.md)
- 04\. [JWT Resource Server](jwt_resource_server.md)
- 05\. [Authorities and Claims](authorities_claims.md)
- 06\. [Method Security](method_security.md)
- 07\. [Password Hashing](password_hashing.md)
- 08\. [CORS and CSRF](cors_csrf.md)
- 09\. [Testing Security](testing_security.md)
- 10\. [Common Mistakes](common_mistakes.md)
- 11\. [Spring Boot JWT and CORS Example](spring_boot_jwt_cors_example.md)

## Suggested Learning Flow

Start with security role, authentication, and authorization. Then study the
security filter chain and JWT resource server setup. After that, review
authorities, method security, password hashing, CORS/CSRF, tests, common
mistakes, and the final implementation example.

## Mini Goal

By the end of this section, you should be able to design a Spring Boot REST API
where:

- public and protected endpoints are explicit;
- JWT validation is handled by Spring Security resource server support;
- authorization rules are checked at the web and method boundaries;
- CORS is configured for known frontend origins;
- CSRF decisions match the authentication mechanism;
- security tests cover unauthorized, forbidden, and allowed requests.

## Interview Readiness

You should be able to answer:

- What is the difference between authentication and authorization?
- What does `SecurityFilterChain` configure?
- Why use OAuth2 Resource Server support for JWT validation?
- How are JWT claims mapped to authorities?
- When is method security useful?
- How should CORS and CSRF be handled in stateless JWT APIs?
- What are common Spring Security mistakes?
