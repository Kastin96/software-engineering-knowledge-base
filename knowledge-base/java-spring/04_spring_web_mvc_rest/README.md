# Spring Web MVC and REST

This section covers REST API development with Spring Web MVC.

The focus is on the HTTP boundary of a backend service: controllers, request
mapping, DTOs, response codes, pagination, filtering, sorting, versioning, and
the separation between web concerns and application behavior.

## Topics

- 01\. [REST Controller Role](rest_controller_role.md)
- 02\. [Request Mapping](request_mapping.md)
- 03\. [Path Variables and Query Parameters](path_variables_query_params.md)
- 04\. [Request Body and Response Body](request_body_response_body.md)
- 05\. [DTO Boundaries](dto_boundaries.md)
- 06\. [Status Codes and ResponseEntity](status_codes_response_entity.md)
- 07\. [Pagination, Filtering, and Sorting](pagination_filtering_sorting.md)
- 08\. [API Versioning](api_versioning.md)
- 09\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of a REST controller and request mapping. Then review how
Spring binds path variables, query parameters, and request bodies. After that,
focus on DTO boundaries, response codes, collection endpoints, versioning, and
common mistakes.

## Mini Goal

By the end of this section, you should be able to design a small REST API where:

- controllers expose HTTP endpoints without owning business rules;
- request and response DTOs are separated from persistence entities;
- path variables and query parameters are used intentionally;
- response status codes reflect API outcomes;
- collection endpoints support pagination and filtering;
- versioning decisions are explicit.

## Interview Readiness

You should be able to answer:

- What should a Spring REST controller be responsible for?
- When should data come from a path variable versus a query parameter?
- Why should entities usually not be exposed directly as API responses?
- When is `ResponseEntity` useful?
- How should pagination be represented in an API?
- What are common REST API design mistakes in Spring applications?
