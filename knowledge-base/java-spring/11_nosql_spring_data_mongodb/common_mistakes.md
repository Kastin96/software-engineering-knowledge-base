# Common Mistakes

MongoDB mistakes often come from treating document storage like relational
storage or assuming Spring abstractions remove data modeling decisions.

## Modeling Documents Like Tables

If every related concept becomes a separate collection, the application may need
many manual joins or cross-document workflows. Model around access patterns.

## Missing Indexes

Queries that work locally can become slow in production without the right
indexes. Design indexes from real filters and sorts.

## Returning Documents From Controllers

Documents are persistence models. Map them to response DTOs before crossing the
HTTP boundary.

## Overusing Transactions

Frequent multi-document transactions can indicate poor document boundaries.
Check whether embedding or workflow design would fit better.

## Saving Stale Full Documents

Replacing a whole document can overwrite fields unintentionally. Use partial
updates when only specific fields should change.

## Untested Aggregations

Aggregation pipelines can become complex query programs. Test them with
realistic data.

## Key Idea

Spring Data MongoDB gives useful infrastructure, but MongoDB success still
depends on document modeling, indexes, and query discipline.
