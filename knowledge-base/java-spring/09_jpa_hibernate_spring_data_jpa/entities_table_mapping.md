# Entities and Table Mapping

An entity represents persistent state mapped to a database table.

JPA entities should be designed as persistence models. They are not automatically
API DTOs and do not have to mirror every domain concept perfectly.

## Basic Entity

```java
@Entity
@Table(name = "orders")
public class OrderEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private Long customerId;

    @Column(nullable = false)
    private String status;

    @Column(nullable = false)
    private BigDecimal total;

    protected OrderEntity() {
        // required by JPA
    }

    public OrderEntity(Long customerId, String status, BigDecimal total) {
        this.customerId = customerId;
        this.status = status;
        this.total = total;
    }
}
```

## Constructor Visibility

JPA requires a no-argument constructor. It can be `protected` to avoid making it
part of the normal application construction API.

## Column Mapping

Use explicit table and column constraints when they communicate persistence
requirements:

```java
@Column(name = "customer_id", nullable = false)
private Long customerId;
```

Database constraints should still exist in migrations. Entity annotations are
not a substitute for database schema control.

## Key Idea

Entities are persistence models. Keep their mapping explicit, and do not rely on
them as public API contracts.
