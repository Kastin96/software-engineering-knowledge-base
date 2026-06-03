# Common Mistakes

Spring Security mistakes often come from copying configuration without matching
it to the application's authentication model.

## Using Deprecated Configuration Style

Modern Spring Security uses component-based configuration with
`SecurityFilterChain`. Avoid old `WebSecurityConfigurerAdapter` examples.

## Writing A Custom JWT Filter Too Early

For standard bearer JWT validation, use OAuth2 Resource Server support. Custom
filters are easy to get wrong.

## Confusing CORS With Security

CORS does not authenticate users. It controls browser-origin access.

## Disabling CSRF Without Understanding Why

Disabling CSRF is usually appropriate for stateless bearer-token APIs. It is not
safe as a blanket rule for browser session applications.

## Using Wildcard CORS In Production

Avoid `*` for production APIs that are intended for known frontends.

## Trusting JWT Claims Without Validation

Never decode a JWT and trust its claims without verifying signature, issuer,
audience when relevant, and token validity.

## Key Idea

Security configuration should match the authentication mechanism, client type,
and deployment model.
