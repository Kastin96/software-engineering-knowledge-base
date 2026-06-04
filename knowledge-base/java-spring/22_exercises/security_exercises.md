# Security Exercises

## Exercise 1: Resource Server

Protect an API with JWT resource server support.

Acceptance criteria:

- public health endpoint remains open;
- protected endpoints require authentication;
- write endpoint requires a write authority;
- tests cover `401`, `403`, and success.

## Exercise 2: CORS

Configure CORS for one frontend origin.

Acceptance criteria:

- allowed origin is explicit;
- methods are limited;
- credentials are handled intentionally;
- preflight behavior is tested or manually verified.

## Exercise 3: Method Security

Protect a service method with authorization.

Acceptance criteria:

- method security is enabled;
- denied access is tested;
- business logic is not duplicated in the controller.
