# MongoDB Role in Spring Applications

MongoDB stores documents rather than rows.

In Spring applications, MongoDB is usually accessed through Spring Data MongoDB:
repositories for common access patterns, `MongoTemplate` for more explicit
operations, and mapping annotations for document classes.

## When MongoDB Fits

MongoDB can fit well when:

- data is naturally document-shaped;
- related data is usually read together;
- schema flexibility is useful;
- aggregate documents can be updated as a unit;
- query patterns are known and can be indexed.

## When To Be Careful

Be careful when the use case depends heavily on:

- complex joins;
- strict relational constraints;
- multi-table reporting;
- cross-aggregate transactions;
- ad hoc query patterns without clear indexes.

MongoDB can support many advanced scenarios, but the data model and indexes must
be designed deliberately.

## Spring Boundary

```text
Service -> MongoRepository / MongoTemplate -> MongoDB
```

The service owns application behavior. The repository or template code owns
persistence details.

## Key Idea

MongoDB is not "SQL without tables". Model documents around access patterns,
consistency needs, and update behavior.
