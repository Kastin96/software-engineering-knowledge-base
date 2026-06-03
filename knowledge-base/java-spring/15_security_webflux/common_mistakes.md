# Common Mistakes

WebFlux Security mistakes usually come from copying servlet security code into a
reactive application.

## Using Servlet Types

Do not use `HttpSecurity` or `SecurityFilterChain` for WebFlux security. Use
`ServerHttpSecurity` and `SecurityWebFilterChain`.

## Blocking In Security Paths

Avoid blocking database or HTTP calls during authentication or authorization.
Blocking work can damage the reactive execution model.

## Custom JWT Filters Too Early

For normal bearer JWT validation, use reactive OAuth2 Resource Server support.

## Wrong CORS Type

For WebFlux, use the reactive CORS configuration source from
`org.springframework.web.cors.reactive`.

## Treating WebFlux As MVC With Mono

Returning `Mono` from controllers is not enough if the security, service, and
persistence layers block internally.

## Key Idea

WebFlux security needs reactive infrastructure end to end. Keep servlet and
reactive security configuration separate.
