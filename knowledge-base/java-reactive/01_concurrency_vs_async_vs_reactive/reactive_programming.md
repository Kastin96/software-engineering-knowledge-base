# Reactive Programming

Reactive programming models work as asynchronous streams with clear signals for
data, errors, completion, and demand.

In Java backend work, reactive programming usually means using Reactive Streams
concepts through a library such as Project Reactor. Spring WebFlux builds on
that model.

## Stream Shape

A reactive pipeline does not hold the final value. It describes how values
should be produced, transformed, combined, and handled when subscribed to.

```java
Mono<OrderView> orderView =
    orderRepository.findById(orderId)
        .zipWith(customerClient.findCustomer(orderId))
        .map(tuple -> OrderView.from(tuple.getT1(), tuple.getT2()))
        .timeout(Duration.ofMillis(500));
```

`Mono<OrderView>` is not an `OrderView`. It is a description of asynchronous
work that may produce one value, fail, or complete empty.

## Reactive Streams Signals

A reactive stream has a small set of important signals:

- next value;
- error;
- completion;
- demand from the subscriber.

Demand is what makes backpressure possible. The consumer can signal how much
work it is ready to receive.

## Good Use Cases

Reactive programming fits when:

- the request path can stay non-blocking;
- dependencies provide reactive clients;
- many concurrent I/O operations must be composed;
- streaming data or backpressure matters;
- the team can maintain reactive pipelines consistently.

## Poor Use Cases

Reactive programming is often a poor fit when:

- most dependencies are JDBC, JPA, or synchronous SDKs;
- the code frequently calls `block()`;
- the team only needs simple CRUD over a blocking database;
- the reactive pipeline hides business behavior.

## Key Idea

Reactive programming is a data-flow and coordination model for asynchronous
streams. It pays off when the whole path supports the model.
