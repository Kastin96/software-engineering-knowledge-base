# Practical Examples

This section collects practical Spring Boot scenarios that combine several
topics from the previous sections.

The goal is to practice design and implementation decisions, not to copy large
code listings. Each example describes the problem, the expected structure, and
the important implementation points.

## Topics

- 01\. [Order REST API](order_rest_api.md)
- 02\. [MongoDB Query Service](mongodb_query_service.md)
- 03\. [Reactive WebFlux Endpoint](reactive_webflux_endpoint.md)
- 04\. [JWT Secured API](jwt_secured_api.md)
- 05\. [Kafka Event Flow](kafka_event_flow.md)
- 06\. [Production Ready Service](production_ready_service.md)
- 07\. [Testing Strategy Example](testing_strategy_example.md)
- 08\. [Refactoring Example](refactoring_example.md)

## How To Use

Pick one example and implement it in a small Spring Boot project. Keep the
implementation focused. The point is to practice boundaries, configuration,
testing, and failure behavior.

## Mini Goal

By the end of this section, you should be able to:

- translate requirements into Spring components;
- decide where controller, service, repository, DTO, and configuration code
  belongs;
- choose between MVC, WebFlux, SQL, MongoDB, and Kafka where relevant;
- add tests that prove behavior without testing every framework detail;
- explain production risks and support signals for the implemented flow.
