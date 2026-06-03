# MongoDB Configuration

Spring Boot can auto-configure MongoDB infrastructure when Spring Data MongoDB
and MongoDB driver dependencies are present.

## Typical Configuration

```yaml
spring:
  data:
    mongodb:
      uri: ${ORDERS_MONGODB_URI}
      database: orders
```

The URI often includes host, port, authentication, and connection options. In
production, the actual URI should come from deployment configuration or a secret
manager.

## Repository Support

Spring Boot can auto-enable MongoDB repositories when the application has Spring
Data MongoDB on the classpath and repository interfaces are in scanned packages.

```java
interface OrderMongoRepository extends MongoRepository<OrderDocument, String> {
}
```

## Template Bean

Boot can provide `MongoTemplate`, which is useful for explicit query, update,
index, and aggregation operations.

```java
@Repository
class OrderMongoDao {
    private final MongoTemplate mongoTemplate;

    OrderMongoDao(MongoTemplate mongoTemplate) {
        this.mongoTemplate = mongoTemplate;
    }
}
```

## Key Idea

Keep MongoDB connection settings externalized and let Spring Boot configure the
common infrastructure unless the application has a concrete reason to customize
it.
