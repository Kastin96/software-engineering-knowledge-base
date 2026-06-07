# Getting Results and Timeouts

`Future.get()` blocks the calling thread until the task completes.

That can be acceptable at a clear boundary, but unbounded waiting is dangerous
in backend services.

## Prefer Bounded Waiting

```java
try {
    CustomerRisk risk = riskFuture.get(300, TimeUnit.MILLISECONDS);
    return Decision.from(risk);
} catch (TimeoutException ex) {
    riskFuture.cancel(true);
    return Decision.manualReview("Risk lookup timed out");
}
```

The timeout should match the caller contract and service-level objectives. A
background task should not outlive the request indefinitely unless that is an
explicit design choice.

## Avoid Waiting Too Early

Submitting tasks and immediately calling `get()` removes most concurrency
benefit.

```java
Future<Customer> customer = executor.submit(() -> customerClient.fetch(id));
Customer value = customer.get(); // blocks before other independent work starts
```

Start independent work first, then join at the boundary.

```java
Future<Customer> customer = executor.submit(() -> customerClient.fetch(id));
Future<List<Order>> orders = executor.submit(() -> orderClient.fetchRecent(id));

CustomerDashboard dashboard = new CustomerDashboard(
    customer.get(300, TimeUnit.MILLISECONDS),
    orders.get(300, TimeUnit.MILLISECONDS)
);
```

## Blocking Thread Cost

Waiting on a `Future` blocks the current thread. In a Spring MVC request thread,
that may be acceptable if bounded. In a WebFlux event-loop thread, it is usually
wrong.

## Key Idea

Use `get(timeout, unit)` at explicit boundaries. Avoid unbounded waits and avoid
blocking event-loop or scheduler threads that must stay non-blocking.
