# Testing Spring Applications

This section covers testing Spring Boot applications without turning every test
into a full application startup.

The goal is to choose the smallest test scope that still verifies the behavior
that matters: service logic, HTTP contracts, persistence mapping, security
rules, reactive pipelines, or end-to-end integration.

## Topics

- 01\. [Testing Strategy](testing_strategy.md)
- 02\. [Unit Testing Services](unit_testing_services.md)
- 03\. [Spring Boot Test Slices](spring_boot_test_slices.md)
- 04\. [Web MVC Controller Tests](web_mvc_controller_tests.md)
- 05\. [Data Access Tests](data_access_tests.md)
- 06\. [Security Tests](security_tests.md)
- 07\. [Integration Tests](integration_tests.md)
- 08\. [Testcontainers](testcontainers.md)
- 09\. [Test Profiles and Configuration](test_profiles_configuration.md)
- 10\. [WebFlux and Reactor Tests](webflux_reactor_tests.md)
- 11\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with testing strategy and unit tests. Then study slice tests, controller
tests, data access tests, and security tests. After that, review integration
tests, Testcontainers, test configuration, WebFlux/Reactor testing, and common
mistakes.

## Mini Goal

By the end of this section, you should be able to design a Spring test suite
where:

- service logic is tested without loading Spring;
- web contracts are tested with focused controller tests;
- repository tests verify real persistence behavior;
- security rules are tested for allowed and denied paths;
- full integration tests are reserved for important flows;
- WebFlux services are tested with `StepVerifier`;
- WebFlux controllers are tested with `WebTestClient`.

## Interview Readiness

You should be able to answer:

- When would you use a unit test instead of `@SpringBootTest`?
- What is a Spring Boot test slice?
- When is `@WebMvcTest` useful?
- Why test repositories against a real database?
- When should Testcontainers be used?
- How do you test a `Mono` or `Flux`?
- How do you test a WebFlux controller?
