# Resilience and Degradation

Production services should fail predictably.

Resilience is not only "retry more". It is timeouts, bounded retries, fallback
behavior, circuit breaking where useful, queues, rate limits, and operational
visibility.

## Degradation Example

```java
public ProductPage loadProductPage(Long productId) {
    Product product = productService.findById(productId);

    try {
        List<Recommendation> recommendations = recommendationClient.findFor(productId);
        return ProductPage.withRecommendations(product, recommendations);
    } catch (RecommendationServiceException ex) {
        log.warn("Recommendations unavailable: productId={}", productId, ex);
        return ProductPage.withoutRecommendations(product);
    }
}
```

This works only if recommendations are optional. Do not silently degrade
required business operations.

## Retry And Backoff

```java
Retry.backoff(3, Duration.ofMillis(200))
    .maxBackoff(Duration.ofSeconds(2));
```

Retries should have limits. Unbounded retries can amplify an outage.

## Bulkheads

Separate critical and non-critical work where possible. One slow dependency
should not consume every thread or connection in the application.

## Key Idea

Production reliability comes from bounded failure. A service should be clear
about what can fail, what can retry, and what can degrade.
