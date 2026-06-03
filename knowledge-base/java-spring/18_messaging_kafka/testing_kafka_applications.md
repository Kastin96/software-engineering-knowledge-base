# Testing Kafka Applications

Unit tests are enough for pure mapping and service logic. Kafka integration
tests are useful when you need to verify actual producer/consumer behavior,
serialization, topic configuration, and listener wiring.

## Testcontainers Example

```java
@SpringBootTest
@Testcontainers
class OrderPaidKafkaIntegrationTest {

    @Container
    static KafkaContainer kafka = new KafkaContainer(
        DockerImageName.parse("apache/kafka-native:3.9.0")
    );

    @DynamicPropertySource
    static void kafkaProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.kafka.bootstrap-servers", kafka::getBootstrapServers);
    }

    @Autowired
    private KafkaTemplate<String, OrderPaidEvent> kafkaTemplate;

    @Autowired
    private InvoiceRepository invoiceRepository;

    @Test
    void consumesOrderPaidEvent() throws Exception {
        OrderPaidEvent event = new OrderPaidEvent(
            "event-1",
            42L,
            100L,
            new BigDecimal("49.90"),
            Instant.now()
        );

        kafkaTemplate.send("orders.paid.v1", "42", event)
            .get(10, TimeUnit.SECONDS);

        await().atMost(Duration.ofSeconds(10))
            .untilAsserted(() ->
                assertThat(invoiceRepository.existsByOrderId(42L)).isTrue()
            );
    }
}
```

This test verifies the real Kafka path: serialization, send, listener
invocation, and database side effect.

## What To Test

Test:

- producer sends to the correct topic with the expected key;
- listener processes valid events;
- invalid events go through configured error handling;
- duplicate events do not create duplicate side effects;
- DLT behavior works for non-retryable failures.

## What Not To Test

Do not use Kafka integration tests for every small mapper or branch. Keep those
as fast unit tests.

## Key Idea

Kafka tests should prove integration behavior. Keep them focused, because broker
tests are slower than normal service tests.
