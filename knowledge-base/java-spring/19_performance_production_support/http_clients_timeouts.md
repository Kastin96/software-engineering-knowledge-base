# HTTP Clients and Timeouts

Every outbound HTTP call should have timeouts.

Without timeouts, a slow downstream service can hold request threads, exhaust
connection pools, and make the whole application look unhealthy.

## RestClient Example

```java
@Bean
RestClient paymentRestClient(RestClient.Builder builder) {
    ClientHttpRequestFactorySettings settings = ClientHttpRequestFactorySettings.DEFAULTS
        .withConnectTimeout(Duration.ofSeconds(2))
        .withReadTimeout(Duration.ofSeconds(5));

    return builder
        .baseUrl("https://payments.example.com")
        .requestFactory(ClientHttpRequestFactories.get(settings))
        .build();
}
```

Timeout values should match the business operation and the upstream service SLO.

## WebClient Example

```java
@Bean
WebClient paymentWebClient(WebClient.Builder builder) {
    HttpClient httpClient = HttpClient.create()
        .responseTimeout(Duration.ofSeconds(5))
        .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 2_000);

    return builder
        .baseUrl("https://payments.example.com")
        .clientConnector(new ReactorClientHttpConnector(httpClient))
        .build();
}
```

Do not use reactive clients as a magic performance fix. If the rest of the flow
blocks, the system is still blocking.

## Retry Rule

Retries can improve resilience for temporary failures, but they also multiply
traffic.

Retry only when:

- the operation is safe to retry;
- there is a timeout;
- there is a backoff;
- the retry count is limited;
- failures are observable.

## Key Idea

Timeouts are not optional production configuration. They are part of the service
contract with every downstream dependency.
