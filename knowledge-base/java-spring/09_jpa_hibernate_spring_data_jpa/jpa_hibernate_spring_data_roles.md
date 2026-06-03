# JPA, Hibernate, and Spring Data Roles

JPA, Hibernate, and Spring Data JPA are related, but they are not the same
thing.

## JPA

JPA is a Java specification for ORM behavior. It defines concepts such as:

- entities;
- persistence context;
- entity manager;
- relationships;
- JPQL;
- transactions around entity state changes.

JPA describes the contract. It does not provide the implementation by itself.

## Hibernate

Hibernate is an ORM implementation and JPA provider. It implements the JPA
contract and adds provider-specific behavior and features.

In many Spring Boot applications, Hibernate is the default JPA provider.

## Spring Data JPA

Spring Data JPA adds repository abstractions on top of JPA:

```java
interface OrderRepository extends JpaRepository<OrderEntity, Long> {
    List<OrderEntity> findByCustomerId(Long customerId);
}
```

It can generate common repository implementations, derive queries from method
names, and integrate with pagination and sorting.

## Practical Boundary

```text
Spring Data JPA repository -> JPA EntityManager -> Hibernate -> database
```

Spring Data reduces repository boilerplate. JPA defines ORM behavior. Hibernate
executes the ORM behavior.

## Key Idea

Spring Data JPA makes common persistence access concise, but the runtime
behavior is still JPA/Hibernate behavior.
