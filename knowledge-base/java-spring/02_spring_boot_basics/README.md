# Spring Boot Basics

This section covers Spring Boot as the production-oriented layer on top of the
Spring ecosystem.

Spring Boot does not replace Spring Core. It reduces setup cost by providing
auto-configuration, dependency starters, embedded server support, externalized
configuration, actuator integration, and executable packaging.

## Topics

- 01\. [What Is Spring Boot](what_is_spring_boot.md)
- 02\. [Project Structure](project_structure.md)
- 03\. [Starters](starters.md)
- 04\. [Auto-Configuration](auto_configuration.md)
- 05\. [Application Properties and YAML](application_properties_yml.md)
- 06\. [Profiles](profiles.md)
- 07\. [Configuration Properties](configuration_properties.md)
- 08\. [Running and Packaging](running_packaging.md)
- 09\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the role of Spring Boot and the default project structure. Then
review starters and auto-configuration, because they explain most Boot behavior.
After that, study externalized configuration, profiles, type-safe configuration
properties, and packaging.

## Mini Goal

By the end of this section, you should be able to set up a small Spring Boot
service that:

- has a clean package structure;
- uses focused starters;
- reads settings from `application.yml`;
- separates local and production configuration with profiles;
- binds grouped settings into a typed properties class;
- runs as an executable jar.

## Interview Readiness

You should be able to answer:

- What problem does Spring Boot solve?
- What is a starter?
- What is auto-configuration?
- How does Boot decide what to configure?
- What is the difference between `application.yml` and profiles?
- When should you use `@ConfigurationProperties` instead of `@Value`?
- How is a Spring Boot application usually packaged and run?
