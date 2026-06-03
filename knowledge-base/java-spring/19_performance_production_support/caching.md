# Caching

Caching can reduce latency and load, but it adds correctness questions.

Use caching when data is read frequently, changes less often, and can tolerate a
defined freshness window.

## Spring Cache Example

```java
@Service
public class ProductCatalogService {
    private final ProductRepository productRepository;

    @Cacheable(cacheNames = "products", key = "#id")
    public ProductDetails findProduct(Long id) {
        return productRepository.findDetailsById(id)
            .orElseThrow(() -> new ProductNotFoundException(id));
    }

    @CacheEvict(cacheNames = "products", key = "#id")
    public void updateProduct(Long id, UpdateProductRequest request) {
        productRepository.update(id, request);
    }
}
```

## Cache Configuration

```yaml
spring:
  cache:
    cache-names: products
```

The actual cache provider matters. In-memory cache is simple but local to one
instance. Redis or another distributed cache may be needed when instances must
share cache state.

## What To Cache

Good candidates:

- reference data;
- product/category metadata;
- configuration-like data;
- expensive read models.

Risky candidates:

- highly personalized data;
- rapidly changing balances or inventory;
- security decisions without clear expiration;
- data with complex invalidation rules.

## Key Idea

Caching is a consistency trade-off. Define the freshness rule before adding the
annotation.
