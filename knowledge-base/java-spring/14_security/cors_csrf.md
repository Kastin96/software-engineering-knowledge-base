# CORS and CSRF

CORS and CSRF solve different problems.

## CORS

CORS controls which browser origins can call the API.

It is enforced by browsers. It is not an authentication or authorization
mechanism.

Spring Security needs CORS to be handled before authentication checks because
browser preflight requests do not contain normal credentials.

## CSRF

CSRF protects browser session-based applications from unwanted state-changing
requests made with automatically attached credentials such as cookies.

For stateless APIs authenticated with bearer tokens in the `Authorization`
header, CSRF is often disabled because the browser does not automatically attach
the bearer token.

## Stateless JWT API Pattern

```java
http
    .csrf(csrf -> csrf.disable())
    .cors(cors -> cors.configurationSource(corsConfigurationSource()))
    .oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()));
```

Do not disable CSRF casually for session-cookie applications.

## CORS Rules

Prefer explicit origins:

```text
https://app.example.com
https://admin.example.com
```

Avoid wide-open production CORS policies unless the API is intentionally public
and does not use browser credentials.

## Key Idea

CORS is browser access control. CSRF is request-forgery protection. Configure
both based on the authentication mechanism.
