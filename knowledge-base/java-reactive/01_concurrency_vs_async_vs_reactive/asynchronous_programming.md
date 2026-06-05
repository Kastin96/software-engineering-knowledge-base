# Asynchronous Programming

Asynchronous programming lets a task start now and complete later without
returning the final value immediately.

In Java, the most common production-oriented abstraction for async composition
outside reactive libraries is `CompletableFuture`.

## CompletableFuture Example

`CompletableFuture` is useful for composing independent asynchronous operations.

```java
CompletableFuture<Customer> customer =
    customerClient.fetchCustomerAsync(customerId);

CompletableFuture<List<Order>> orders =
    orderClient.fetchRecentOrdersAsync(customerId);

CompletableFuture<CustomerDashboard> dashboard =
    customer.thenCombine(orders, CustomerDashboard::new)
        .orTimeout(500, TimeUnit.MILLISECONDS);
```

The code describes two independent calls and combines their results. The caller
receives a future, not the final dashboard immediately.

## Good Use Cases

Asynchronous code fits when:

- independent I/O calls can run at the same time;
- the caller can continue without the result;
- a workflow needs explicit composition;
- timeout and fallback behavior must be attached to the operation.

## Risks

Async code can become difficult to maintain when:

- execution pools are implicit or poorly sized;
- exception handling is inconsistent;
- cancellation is ignored;
- blocking calls are hidden inside async tasks;
- nested callbacks or futures make the flow hard to read.

## Async Is Not Automatically Non-Blocking

Running blocking code on another thread is asynchronous from the caller's
perspective, but the work still blocks a thread.

That can be perfectly acceptable. It just should not be confused with a
non-blocking I/O model.

## Key Idea

Asynchronous programming changes when and how results are delivered. It does not
automatically make the underlying operation non-blocking or reactive.
