# Callable and Future

This section covers `Callable` and `Future` as the classic Java model for
running work asynchronously and retrieving a result later.

`Callable` adds a result and checked exception support to task execution.
`Future` represents the pending result of that task. Together, they are useful
for bounded background work, request fan-out, batch processing, and integration
with older Java APIs.

## Topics

- 01\. [`Callable`](callable.md)
- 02\. [`Future`](future.md)
- 03\. [Getting Results and Timeouts](getting_results_timeouts.md)
- 04\. [Exceptions](exceptions.md)
- 05\. [Cancellation](cancellation.md)
- 06\. [`invokeAll` and `invokeAny`](invoke_all_invoke_any.md)
- 07\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with `Callable` and `Future`. Then review blocking result retrieval,
timeouts, exceptions, and cancellation. Finish with batch execution APIs and
common mistakes before moving to `CompletableFuture`.

## Mini Goal

By the end of this section, you should be able to design a bounded async
workflow where:

- each task returns a useful result;
- callers use timeouts instead of waiting indefinitely;
- failures are unwrapped and handled intentionally;
- cancellation is interruption-aware;
- batch execution does not hide partial failures.

## Interview Readiness

You should be able to answer:

- What is the difference between `Runnable` and `Callable`?
- What does `Future` represent?
- Why can `Future.get()` be dangerous without a timeout?
- How are task exceptions exposed through `Future`?
- What does `cancel(true)` actually do?
- Why is `CompletableFuture` often preferred for composition?
