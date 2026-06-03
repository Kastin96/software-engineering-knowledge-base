# Integration Design

Spring services usually integrate through REST, messaging, databases, or
third-party APIs.

The integration style should match the business need.

## REST

REST is useful when:

- the caller needs an immediate answer;
- the operation is request/response;
- the dependency is required to complete the use case;
- the API contract is stable and explicit.

## Messaging

Messaging is useful when:

- consumers can process asynchronously;
- multiple systems need the same event;
- replay is useful;
- the producer should not wait for every consumer;
- eventual consistency is acceptable.

## Database Sharing

Avoid direct database sharing between services unless there is a strong reason.
Shared databases blur ownership and make independent change harder.

## Anti-Corruption Layer

Wrap external API details behind a local interface.

```java
public interface PaymentGateway {
    PaymentConfirmation charge(Order order, PaymentRequest request);
}
```

The application service should not be full of third-party DTOs and protocol
details.

## Key Idea

Integration design is about coupling. Choose the style that gives the required
consistency without making every system depend on every other system.
