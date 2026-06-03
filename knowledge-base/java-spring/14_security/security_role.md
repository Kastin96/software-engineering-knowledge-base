# Security Role

Security defines who can call the application and what they are allowed to do.

In a backend service, Spring Security usually sits at the HTTP boundary and
enforces authentication and authorization before controller code runs.

## Common Responsibilities

Spring Security commonly handles:

- authentication through sessions, bearer tokens, or OAuth2/OIDC;
- authorization by roles, authorities, scopes, or policies;
- password hashing for local credentials;
- CSRF protection for browser session flows;
- CORS integration for browser-based clients;
- security headers;
- method-level authorization;
- standardized unauthorized and forbidden responses.

## REST API Context

For REST APIs, a common modern setup is:

```text
Frontend or client -> Bearer JWT -> Spring Security Resource Server -> Controller
```

The API validates the token and authorizes the request. It does not need to parse
JWTs manually in every controller.

## Security Is A Boundary

Controllers should not contain repeated checks such as:

```java
if (!request.user().hasRole("ADMIN")) {
    throw new ForbiddenException();
}
```

Use security configuration and method security where possible.

## Key Idea

Security should be centralized enough to be consistent, but explicit enough that
endpoint access rules are reviewable.
