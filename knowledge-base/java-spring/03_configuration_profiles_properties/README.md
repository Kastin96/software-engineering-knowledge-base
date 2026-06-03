# Configuration, Profiles, and Properties

This section covers configuration as an operational boundary in Spring Boot
applications.

Spring Boot makes configuration easy to add, but production services need more
than convenient property files. Configuration should be typed, validated,
environment-aware, and safe to change without hiding business behavior in YAML.

## Topics

- 01\. [Configuration Model](configuration_model.md)
- 02\. [Property Sources and Precedence](property_sources_precedence.md)
- 03\. [Profiles in Practice](profiles_in_practice.md)
- 04\. [Environment Variables](environment_variables.md)
- 05\. [Type-Safe Configuration](type_safe_configuration.md)
- 06\. [Configuration Validation](configuration_validation.md)
- 07\. [Secrets and Sensitive Values](secrets_sensitive_values.md)
- 08\. [Feature Flags](feature_flags.md)
- 09\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with Spring Boot's configuration model and property source precedence.
Then review profiles and environment variables. After that, focus on typed
configuration, validation, secrets, feature flags, and common mistakes.

## Mini Goal

By the end of this section, you should be able to design configuration for a
service that:

- separates local, test, and production settings;
- uses environment variables for deployment-specific values;
- binds grouped settings into typed classes;
- validates required configuration at startup;
- avoids committing secrets;
- keeps business rules out of property files.

## Interview Readiness

You should be able to answer:

- How does Spring Boot load configuration?
- What happens when the same property is defined in multiple places?
- How should profiles be used in real services?
- Why is `@ConfigurationProperties` usually better for grouped settings?
- How can configuration be validated at startup?
- Where should secrets come from?
- What are the risks of feature flags?
