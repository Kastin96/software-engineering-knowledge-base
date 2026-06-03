# Error Handling

This section covers error handling in Spring MVC REST applications.

Error handling is part of the API contract. A backend service should translate
internal failures into consistent HTTP responses without leaking stack traces,
database details, framework exceptions, or inconsistent message formats.

## Topics

- 01\. [Error Handling Role](error_handling_role.md)
- 02\. [Exception Types](exception_types.md)
- 03\. [Global Exception Handler](global_exception_handler.md)
- 04\. [Problem Details](problem_details.md)
- 05\. [Validation Error Responses](validation_error_responses.md)
- 06\. [Domain and Application Exceptions](domain_application_exceptions.md)
- 07\. [HTTP Status Mapping](http_status_mapping.md)
- 08\. [Exception Logging](exception_logging.md)
- 09\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of error handling and exception categories. Then study
global handlers and response formats. After that, focus on validation failures,
domain/application exceptions, status mapping, logging, and common mistakes.

## Mini Goal

By the end of this section, you should be able to design an error handling layer
where:

- controllers do not manually catch common exceptions;
- API errors have a consistent response shape;
- validation errors include useful field-level details;
- domain exceptions map to clear HTTP statuses;
- infrastructure failures are not exposed to clients;
- logs contain diagnostic context without leaking sensitive data.

## Interview Readiness

You should be able to answer:

- Why should REST APIs have a consistent error format?
- What does `@ControllerAdvice` do?
- How should validation errors differ from business errors?
- When should an exception become `404`, `409`, or `400`?
- Why is logging every exception at `ERROR` a problem?
- What details should not be returned to API clients?
