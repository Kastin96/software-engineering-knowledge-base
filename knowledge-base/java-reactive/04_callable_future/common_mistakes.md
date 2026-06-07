# Common Mistakes

`Callable` and `Future` are straightforward APIs, but their blocking and failure
behavior can cause production issues.

## Calling get Without Timeout

Unbounded `get()` can block request threads, shutdown hooks, workers, or
scheduled tasks indefinitely.

Prefer `get(timeout, unit)` unless the wait is intentionally unbounded.

## Ignoring ExecutionException Cause

`ExecutionException` wraps the real task failure.

Logging only the wrapper often hides the useful exception type and stack trace.

## Swallowing InterruptedException

Waiting code should restore interruption.

```java
catch (InterruptedException ex) {
    Thread.currentThread().interrupt();
    throw new ServiceUnavailableException("Interrupted", ex);
}
```

## Assuming Timeout Stops Work

A timeout stops waiting. It does not automatically stop the running task.

Cancel the future when continued work is not useful.

## Blocking In Reactive Pipelines

Calling `Future.get()` inside a Reactor pipeline or WebFlux event-loop path
breaks the non-blocking model.

Use a compatible reactive API or isolate blocking work deliberately on a bounded
scheduler.

## Using Future For Complex Composition

`Future` is weak for chaining, combining, fallback logic, and callbacks.

When a workflow needs composition, move to `CompletableFuture` or a reactive
library instead of building manual orchestration around `get()`.

## Key Idea

`Future` is a low-level result handle. Use it with explicit timeouts,
interruption handling, cancellation policy, and clear composition boundaries.
