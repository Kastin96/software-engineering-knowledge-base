# Architecture Mindset

Architecture is about decisions that are expensive to change later.

In Spring applications, those decisions usually include:

- module boundaries;
- data ownership;
- integration style;
- transaction boundaries;
- API contracts;
- where business rules live;
- how the service is tested and operated.

## Good Architecture

Good architecture makes normal changes easier:

- adding a field to an API response;
- changing a persistence query;
- adding a business rule;
- introducing a new integration;
- testing a use case without starting the whole app.

## Avoid Architecture Theater

Do not add patterns because they sound senior. Add structure when it solves a
real problem: duplicated logic, unclear ownership, hard testing, unstable APIs,
or risky changes.

## Practical Rule

Start simple, but leave boundaries visible.

```text
controller -> application service -> domain/persistence
```

This is enough for many services. Add more abstraction only when the codebase
earns it.

## Key Idea

Architecture should reduce change cost. If it only adds ceremony, it is not
helping.
