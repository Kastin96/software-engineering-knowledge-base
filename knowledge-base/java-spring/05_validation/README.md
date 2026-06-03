# Validation

This section covers validation in Spring applications, especially request
validation at the HTTP boundary.

Validation should reject structurally invalid input early, before the
application service starts executing business behavior. It should also stay
separate from domain rules that require state, persistence, permissions, or
cross-aggregate decisions.

## Topics

- 01\. [Validation Role](validation_role.md)
- 02\. [Bean Validation Basics](bean_validation_basics.md)
- 03\. [Request Body Validation](request_body_validation.md)
- 04\. [Path and Query Parameter Validation](path_query_parameter_validation.md)
- 05\. [Nested DTO Validation](nested_dto_validation.md)
- 06\. [Custom Constraints](custom_constraints.md)
- 07\. [Validation Groups](validation_groups.md)
- 08\. [Service-Level Validation](service_level_validation.md)
- 09\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of validation and Bean Validation basics. Then study request
body validation, path/query validation, and nested DTOs. After that, review
custom constraints, groups, service-level validation, and common mistakes.

## Mini Goal

By the end of this section, you should be able to design validation for a REST
endpoint where:

- request DTOs express structural constraints;
- nested objects are validated correctly;
- path and query parameters are constrained;
- custom constraints are used only when they clarify the contract;
- business rules stay in services or domain code;
- validation failures can be converted into consistent API errors.

## Interview Readiness

You should be able to answer:

- What kind of rules belong in request validation?
- What does `@Valid` do in a Spring controller?
- How do you validate nested DTOs?
- When would you write a custom constraint?
- What are validation groups used for?
- Why should database-dependent business rules usually not live in annotations?
