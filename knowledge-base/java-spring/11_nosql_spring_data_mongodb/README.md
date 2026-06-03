# NoSQL with Spring Data MongoDB

This section covers MongoDB access with Spring Data MongoDB.

MongoDB is a document database. Spring Data MongoDB provides object mapping,
repository support, `MongoTemplate`, query APIs, aggregation support, index
management, exception translation, and transaction integration. It keeps the
Spring programming model familiar while preserving MongoDB-specific design
concerns.

## Topics

- 01\. [MongoDB Role in Spring Applications](mongodb_role.md)
- 02\. [Document Model and Mapping](document_model_mapping.md)
- 03\. [MongoDB Configuration](mongodb_configuration.md)
- 04\. [MongoRepository](mongo_repository.md)
- 05\. [MongoTemplate](mongo_template.md)
- 06\. [Query Methods and Criteria](query_methods_criteria.md)
- 07\. [Indexes](indexes.md)
- 08\. [Aggregation Pipelines](aggregation_pipelines.md)
- 09\. [Updates and Partial Changes](updates_partial_changes.md)
- 10\. [Transactions and Consistency](transactions_consistency.md)
- 11\. [Testing MongoDB Repositories](testing_mongodb_repositories.md)
- 12\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with MongoDB's role and document mapping. Then review configuration,
repositories, `MongoTemplate`, and query APIs. After that, study indexes,
aggregation, updates, transactions, repository tests, and common mistakes.

## Mini Goal

By the end of this section, you should be able to design a MongoDB persistence
layer where:

- document shape follows query and update patterns;
- Spring Data mapping is explicit enough to review;
- repositories are used for simple access patterns;
- `MongoTemplate` is used for complex queries and updates;
- indexes match real query paths;
- aggregation pipelines are treated as production query logic;
- consistency expectations are documented.

## Interview Readiness

You should be able to answer:

- How is MongoDB modeling different from relational modeling?
- What does `@Document` do?
- When would you use `MongoRepository` versus `MongoTemplate`?
- Why are indexes critical for MongoDB queries?
- What is an aggregation pipeline?
- When are MongoDB transactions appropriate?
- How would you test Spring Data MongoDB code?
