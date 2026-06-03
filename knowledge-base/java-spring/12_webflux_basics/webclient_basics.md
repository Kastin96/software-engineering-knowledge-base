# WebClient Basics

`WebClient` is Spring's reactive HTTP client.

It can be used in WebFlux services and also in Spring MVC services that want a
modern client for outbound HTTP calls.

## Configuration

```java
@Configuration
class WebClientConfig {
    @Bean
    WebClient paymentWebClient(WebClient.Builder builder, PaymentProperties properties) {
        return builder
            .baseUrl(properties.baseUrl().toString())
            .build();
    }
}
```

## GET Request

```java
Mono<PaymentResponse> findPayment(String paymentId) {
    return paymentWebClient
        .get()
        .uri("/payments/{id}", paymentId)
        .retrieve()
        .bodyToMono(PaymentResponse.class);
}
```

## Error Mapping

```java
Mono<PaymentResponse> findPayment(String paymentId) {
    return paymentWebClient
        .get()
        .uri("/payments/{id}", paymentId)
        .retrieve()
        .onStatus(HttpStatusCode::is4xxClientError,
            response -> Mono.error(new PaymentClientException(paymentId)))
        .bodyToMono(PaymentResponse.class);
}
```

## Avoid Blocking

In WebFlux code, avoid:

```java
paymentWebClient.get().uri("/payments/{id}", id).retrieve().bodyToMono(PaymentResponse.class).block();
```

Return and compose the `Mono` instead.

## Key Idea

`WebClient` is most valuable when its reactive result is composed through the
application path instead of immediately blocked.
