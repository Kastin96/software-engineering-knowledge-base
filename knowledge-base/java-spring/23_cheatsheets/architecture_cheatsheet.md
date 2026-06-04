# Architecture Cheatsheet

## Boundaries

- Controller: HTTP boundary.
- Application service: use case and transaction boundary.
- Domain: business state and rules.
- Repository: persistence boundary.
- DTO: external contract.
- Event: asynchronous integration contract.

## Good Signs

- controllers are thin;
- business logic is easy to find;
- DTOs protect API contracts;
- transaction boundaries are visible;
- packages show ownership;
- tests can target behavior at the right level.

## Trade-Offs

- REST is direct but couples latency and availability.
- Kafka decouples consumers but adds delivery and idempotency concerns.
- More abstraction can help change, but can also slow development.
- Rich domain models help complex rules; simple scripts may fit CRUD.

## Quick Questions

- What can change independently?
- Who owns this data?
- Where does consistency need to hold?
- What is the simplest design that keeps boundaries clear?
