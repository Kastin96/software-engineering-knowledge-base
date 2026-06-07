# invokeAll and invokeAny

`ExecutorService` provides batch execution methods for groups of `Callable`
tasks.

They are useful for small bounded groups of work, but they still require careful
timeout and failure handling.

## invokeAll

`invokeAll` submits all tasks and returns a `List<Future<T>>`.

```java
List<Callable<InventoryResult>> tasks = warehouses.stream()
    .map(warehouse -> (Callable<InventoryResult>) () ->
        inventoryClient.lookup(warehouse, sku)
    )
    .toList();

List<Future<InventoryResult>> futures =
    executor.invokeAll(tasks, 500, TimeUnit.MILLISECONDS);
```

With a timeout, unfinished tasks are cancelled.

Each future still needs to be inspected. A completed future may contain a
failure.

## invokeAny

`invokeAny` returns the first successful result.

```java
PriceQuote quote = executor.invokeAny(
    quoteProviders,
    300,
    TimeUnit.MILLISECONDS
);
```

This can fit fallback provider lookups where any successful response is enough.

## Practical Risks

Batch APIs can hide important details:

- partial failures;
- cancelled tasks;
- slow providers;
- queue saturation;
- unbounded input size;
- missing per-call timeouts.

Do not pass large unbounded collections directly into batch execution.

## Key Idea

`invokeAll` and `invokeAny` are useful for bounded task groups. They are not a
replacement for explicit workflow design, observability, and backpressure.
