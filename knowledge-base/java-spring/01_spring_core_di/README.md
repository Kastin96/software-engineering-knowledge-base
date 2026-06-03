# Spring Core and Dependency Injection

This section covers the foundation of Spring applications: the IoC container,
beans, dependency injection, component scanning, and Java-based configuration.

In typical backend services, Spring is responsible for assembling the object
graph: controllers, services, repositories, clients, validators, configuration
objects, and infrastructure adapters. Application code should still own the
domain decisions; Spring should mainly manage wiring and integration.

## Topics

- 01\. [What Is Spring](what_is_spring.md)
- 02\. [Dependency Injection](dependency_injection.md)
- 03\. [Beans](beans.md)
- 04\. [Application Context](application_context.md)
- 05\. [Configuration](configuration.md)
- 06\. [Component Scan](component_scan.md)
- 07\. [Bean Scopes and Lifecycle](bean_scopes_lifecycle.md)
- 08\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of Spring in application architecture. Then review
dependency injection and bean registration. After that, study the application
context, configuration, component scanning, scopes, lifecycle, and common design
mistakes.

## Mini Goal

By the end of this section, you should be able to describe and implement a
small Spring service where:

- a controller delegates to an application service;
- the service depends on a repository or client abstraction;
- dependencies are constructor-injected;
- infrastructure objects are configured explicitly;
- singleton beans do not keep request-specific mutable state.

## Interview Readiness

You should be able to answer:

- What problem does Spring solve?
- What is dependency injection?
- What is a Spring bean?
- What is the application context?
- Why is constructor injection usually preferred?
- What is the difference between `@Component`, `@Service`, and `@Repository`?
- When would you use `@Bean` instead of `@Component`?
