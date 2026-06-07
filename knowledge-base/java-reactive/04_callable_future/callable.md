# Callable

`Callable<V>` represents a unit of work that returns a value and can throw a
checked exception.

It is the result-producing counterpart to `Runnable`.

## Basic Shape

```java
Callable<CreditDecision> task = () ->
    creditClient.evaluate(applicationId);

Future<CreditDecision> result = executor.submit(task);
```

The task can be executed by an `ExecutorService`. The caller receives a `Future`
that can later provide the result.

## Why Callable Exists

`Runnable` is enough when the task only performs side effects.

`Callable` is better when the caller needs:

- a returned value;
- checked exception support;
- a `Future` result handle;
- timeout and cancellation control.

## Production-Oriented Task

```java
final class PricingLookup implements Callable<PriceQuote> {
    private final PricingClient pricingClient;
    private final String productId;

    PricingLookup(PricingClient pricingClient, String productId) {
        this.pricingClient = pricingClient;
        this.productId = productId;
    }

    @Override
    public PriceQuote call() throws PricingException {
        return pricingClient.fetchQuote(productId);
    }
}
```

The task owns the work, not thread management. The executor owns scheduling.

## Keep Boundaries Clear

A `Callable` should not create its own executor, hide retries without policy, or
swallow exceptions that the caller needs to observe.

## Key Idea

Use `Callable` when asynchronous work has a meaningful result or failure that
the caller must handle.
