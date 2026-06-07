# Future

`Future<V>` is a handle to a result that may not be available yet.

It is returned by executor methods such as `submit`, `invokeAll`, and
`invokeAny`. It supports checking completion, waiting for the result, and
requesting cancellation.

## Basic Usage

```java
Future<CustomerRisk> riskFuture = executor.submit(() ->
    riskClient.calculateRisk(customerId)
);
```

The `Future` does not contain the result immediately. It represents a task that
is running, waiting, completed, failed, or cancelled.

## Useful Methods

- `isDone()`: checks whether the task completed, failed, or was cancelled.
- `isCancelled()`: checks whether cancellation succeeded.
- `get()`: waits until the result is available.
- `get(timeout, unit)`: waits for a bounded amount of time.
- `cancel(mayInterruptIfRunning)`: requests cancellation.

## Future Is Not A Composition API

`Future` is intentionally limited. It does not provide fluent transformations,
combining, fallback composition, or completion callbacks.

For composed workflows, `CompletableFuture` is usually a better fit.

## Good Fit

`Future` fits well when:

- a small number of tasks are submitted;
- the caller waits at a clear boundary;
- timeouts are explicit;
- cancellation behavior is understood;
- composition needs are minimal.

## Key Idea

`Future` is a result handle, not a reactive stream and not a rich async workflow
API.
