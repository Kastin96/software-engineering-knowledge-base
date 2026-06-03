# Spring Core and Dependency Injection

This section explains the foundation of Spring: the container, beans, and
dependency injection.

Spring is useful because backend applications usually have many objects that
need to work together: controllers, services, repositories, clients,
configuration classes, validators, and more. Creating and wiring all of them by
hand becomes repetitive and error-prone. Spring keeps that wiring in one place:
the application context.

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

Start with what Spring is trying to solve. Then learn dependency injection,
because it is the main idea behind most Spring code. After that, study beans and
the application context, then configuration and component scan. Finish with
scopes, lifecycle, and common mistakes.

## Mini Goal

By the end of this section, you should be able to explain and write a small
Spring-style service where:

- a controller depends on a service;
- the service depends on a repository;
- dependencies are passed through constructors;
- Spring creates and wires the objects;
- configuration is explicit enough to understand.

## Interview Readiness

You should be able to answer:

- What problem does Spring solve?
- What is dependency injection?
- What is a Spring bean?
- What is the application context?
- Why is constructor injection usually preferred?
- What is the difference between `@Component`, `@Service`, and `@Repository`?
- When would you use `@Bean` instead of `@Component`?
