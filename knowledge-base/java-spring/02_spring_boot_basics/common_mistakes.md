# Common Mistakes

These mistakes are common when Spring Boot defaults are used without checking
what they activate.

## Adding Too Many Starters

Every starter changes the dependency graph and may trigger auto-configuration.
Keep starters aligned with actual application requirements.

## Treating Auto-Configuration As Magic

Auto-configuration is conditional behavior. If the application starts with
unexpected beans, check dependencies, profiles, properties, and custom bean
definitions.

## Mixing Local And Production Configuration

Local defaults are useful, but production settings should not be committed as
plain secrets or hardcoded URLs.

Use environment variables, deployment configuration, or secret management.

## Overusing `@Value`

Many scattered `@Value` fields make configuration hard to audit.

Prefer `@ConfigurationProperties` for grouped settings such as client URLs,
timeouts, retry limits, and feature flags.

## Putting Business Decisions In YAML

Configuration should tune environment-specific behavior. Core business rules
should be represented in code where they can be tested and reviewed.

## Key Idea

Spring Boot defaults are useful when they are understood. Treat starters,
profiles, and auto-configuration as application design inputs, not background
noise.
