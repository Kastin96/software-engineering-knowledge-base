# Architecture and Design

This section covers practical architecture decisions for Spring Boot services:
layers, boundaries, DTOs, transactions, modules, integration style, and
maintainability.

The goal is not to memorize architecture labels. The goal is to design services
that are understandable, testable, and changeable.

## Topics

- 01\. [Architecture Mindset](architecture_mindset.md)
- 02\. [Layered Architecture](layered_architecture.md)
- 03\. [Domain Logic and Use Cases](domain_logic_use_cases.md)
- 04\. [DTOs, Entities, and Boundaries](dtos_entities_boundaries.md)
- 05\. [Package Structure and Modules](package_structure_modules.md)
- 06\. [Transactions and Consistency Boundaries](transactions_consistency_boundaries.md)
- 07\. [Integration Design](integration_design.md)
- 08\. [API Design Decisions](api_design_decisions.md)
- 09\. [Design Trade-Offs](design_tradeoffs.md)
- 10\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with architecture mindset and layers. Then study where domain logic
belongs, how DTOs protect boundaries, and how packages shape maintainability.
Finish with transactions, integrations, API decisions, and trade-offs.

## Mini Goal

By the end of this section, you should be able to:

- design a Spring service with clear layers;
- keep controllers thin and use cases explicit;
- separate API DTOs from persistence entities;
- choose package structure intentionally;
- explain transaction boundaries;
- decide when to use synchronous or asynchronous integration;
- discuss architecture trade-offs without over-engineering.

## Interview Readiness

You should be able to answer:

- Where should business logic live in a Spring application?
- Why should controllers not contain complex workflows?
- Why are DTOs useful even when entities look similar?
- What is a transaction boundary?
- How do you decide between REST and messaging?
- What makes a module boundary healthy?
- How do you avoid over-engineering a small service?
