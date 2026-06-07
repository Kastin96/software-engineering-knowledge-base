# Exceptions

Exceptions thrown by a `Callable` are captured by the `Future`.

They are not thrown when the task fails. They are rethrown when the caller calls
`get()`, wrapped in `ExecutionException`.

## Handling ExecutionException

```java
try {
    PaymentStatus status = paymentFuture.get(500, TimeUnit.MILLISECONDS);
    return PaymentResponse.from(status);
} catch (ExecutionException ex) {
    Throwable cause = ex.getCause();
    logger.error("Payment status lookup failed", cause);
    return PaymentResponse.unavailable();
}
```

The useful exception is usually `ex.getCause()`.

## InterruptedException

`get()` can throw `InterruptedException` if the waiting thread is interrupted.
Restore the interrupt status and leave the operation.

```java
try {
    return future.get(500, TimeUnit.MILLISECONDS);
} catch (InterruptedException ex) {
    Thread.currentThread().interrupt();
    throw new ServiceUnavailableException("Interrupted while waiting", ex);
}
```

## TimeoutException

`TimeoutException` means the caller stopped waiting. It does not automatically
stop the task.

Cancel the future if the task should not continue.

```java
catch (TimeoutException ex) {
    future.cancel(true);
    throw new ServiceUnavailableException("Timed out waiting for dependency", ex);
}
```

## Avoid Broad Fallbacks

Do not turn all failures into empty success responses. A timeout, validation
error, dependency outage, and programming bug should not be handled identically.

## Key Idea

`Future` failure handling is explicit at `get()`. Unwrap failures, preserve
interruption, and make timeout behavior intentional.
