# Auditing and Timestamps

Persistence models often need audit fields such as creation and update times.

Spring Data JPA can populate common audit fields when auditing is enabled.

## Entity Fields

```java
@CreatedDate
@Column(nullable = false, updatable = false)
private Instant createdAt;

@LastModifiedDate
@Column(nullable = false)
private Instant updatedAt;
```

## Enable Auditing

```java
@EnableJpaAuditing
@SpringBootApplication
public class OrdersApplication {
}
```

Depending on the setup, entities may also need an auditing listener.

## Use UTC

Use a consistent time basis, usually UTC, for persisted timestamps. Avoid mixing
server-local time zones with database time zones without a clear policy.

## Database Defaults

Database defaults and application auditing can both be valid. Be clear about
which layer owns timestamp generation to avoid mismatches.

## Key Idea

Audit fields are operationally important. Define where they are generated and
keep time handling consistent.
