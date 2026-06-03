# JPA, Hibernate, and Spring Data JPA

This section covers relational persistence with JPA, Hibernate, and Spring Data
JPA.

JPA is an ORM specification. Hibernate is the most common JPA provider in Spring
Boot applications. Spring Data JPA adds repository abstractions on top of JPA.
Together they can reduce repetitive data access code, but they also introduce
entity lifecycle, transaction, fetching, and query behavior that must be
understood.

## Topics

- 01\. [JPA, Hibernate, and Spring Data Roles](jpa_hibernate_spring_data_roles.md)
- 02\. [Entities and Table Mapping](entities_table_mapping.md)
- 03\. [Identifiers and Generated Values](identifiers_generated_values.md)
- 04\. [Repositories](repositories.md)
- 05\. [Entity Relationships](entity_relationships.md)
- 06\. [Fetching and Lazy Loading](fetching_lazy_loading.md)
- 07\. [Queries with JPQL and Native SQL](queries_jpql_native_sql.md)
- 08\. [Projections and DTO Queries](projections_dto_queries.md)
- 09\. [N+1 Problem](n_plus_one_problem.md)
- 10\. [Auditing and Timestamps](auditing_timestamps.md)
- 11\. [Testing JPA Repositories](testing_jpa_repositories.md)
- 12\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with the roles of JPA, Hibernate, and Spring Data JPA. Then study entity
mapping, identifiers, repositories, and relationships. After that, focus on
fetching, queries, projections, N+1, auditing, tests, and common mistakes.

## Mini Goal

By the end of this section, you should be able to design a JPA persistence layer
where:

- entities represent persistence state intentionally;
- repositories expose use-case-friendly operations;
- relationships are mapped without accidentally loading large graphs;
- query behavior is visible and testable;
- DTOs and projections are used when API shape differs from entity shape;
- common Hibernate performance traps are recognized early.

## Interview Readiness

You should be able to answer:

- What is the difference between JPA, Hibernate, and Spring Data JPA?
- What is a persistence context?
- What does lazy loading mean?
- What causes the N+1 problem?
- When would you use JPQL, derived queries, or native SQL?
- Why should entities usually not be returned directly from REST controllers?
- How would you test a JPA repository?
