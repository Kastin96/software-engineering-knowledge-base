# Authentication and Authorization

Authentication verifies identity. Authorization verifies access.

They are related, but they answer different questions.

## Authentication

Authentication asks:

```text
Who is making the request?
```

Examples:

- user logged in with a session;
- API client sends a bearer token;
- service account authenticates through OAuth2 client credentials;
- request has a valid JWT issued by a trusted identity provider.

## Authorization

Authorization asks:

```text
What is this authenticated principal allowed to do?
```

Examples:

- any authenticated user can read their own profile;
- only admins can manage users;
- only users with `orders:write` can create orders;
- only the owner can update a resource.

## Roles, Authorities, and Scopes

In Spring Security, authorization commonly uses authorities:

```text
ROLE_ADMIN
SCOPE_orders:read
SCOPE_orders:write
```

Roles are often represented with the `ROLE_` prefix. OAuth2 scopes are commonly
mapped to `SCOPE_` authorities.

## Key Idea

Authentication establishes the principal. Authorization decides whether that
principal can perform the requested action.
