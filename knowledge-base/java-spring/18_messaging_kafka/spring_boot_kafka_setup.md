# Spring Boot Kafka Setup

Spring Boot auto-configures Kafka support when Spring Kafka is on the classpath.

## Dependency

```groovy
implementation "org.springframework.kafka:spring-kafka"
testImplementation "org.springframework.kafka:spring-kafka-test"
```

## Basic Configuration

```yaml
spring:
  kafka:
    bootstrap-servers: localhost:9092
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
    consumer:
      group-id: order-service
      auto-offset-reset: earliest
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
      properties:
        spring.json.trusted.packages: com.example.orders.events
```

Use explicit serializers and deserializers. Relying on defaults makes the setup
harder to reason about when payloads evolve.

## Topic Bean

```java
@Configuration
public class KafkaTopicsConfig {

    @Bean
    NewTopic orderPaidTopic() {
        return TopicBuilder.name("orders.paid.v1")
            .partitions(3)
            .replicas(1)
            .build();
    }
}
```

Topic creation from the application is useful for local development and simple
environments. In mature production setups, topics are often managed by platform
automation.

## Naming

Prefer stable names:

```text
orders.paid.v1
payments.failed.v1
customers.email-changed.v1
```

Version the topic or event contract intentionally. Do not silently break
existing consumers.

## Key Idea

Spring Boot removes a lot of Kafka boilerplate, but the service still needs
clear serializers, group ids, topic names, and environment-specific broker
configuration.
