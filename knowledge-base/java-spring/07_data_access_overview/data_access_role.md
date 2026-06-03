# Data Access Role

The data access layer is the boundary between application behavior and
persistence infrastructure.

It should hide how data is stored while still making important persistence
constraints visible to the service layer.

## Typical Flow

```text
Controller -> Service -> Repository/DAO -> Database
```

The controller handles HTTP. The service owns the use case and transaction
boundary. The repository or DAO owns persistence-specific access.

## Repository Responsibility

A repository should usually handle:

- loading records or aggregates;
- saving changes;
- executing persistence queries;
- mapping database results to persistence/domain objects;
- hiding SQL, ORM, or driver-specific details.

It should not own high-level workflow decisions such as whether an order can be
cancelled.

## Service Responsibility

A service should usually handle:

- use-case orchestration;
- transaction scope;
- permission-sensitive decisions;
- business state transitions;
- combining multiple repositories or clients.

## Key Idea

Data access code should make persistence operations explicit without pushing
database details into controllers or business workflows.
