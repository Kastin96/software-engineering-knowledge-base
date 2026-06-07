# Common Mistakes

Executor mistakes usually come from missing limits or unclear failure handling.

## Using Executors Factory Methods Blindly

`Executors.newFixedThreadPool` uses an unbounded queue.
`Executors.newCachedThreadPool` can create an unbounded number of threads.

Both can be fine in controlled cases, but backend services often need explicit
queue capacity, naming, metrics, and rejection behavior.

## Ignoring Returned Futures

`submit` captures exceptions in the returned `Future`.

If the future is ignored, failures can disappear.

## No Shutdown Path

An executor without shutdown can keep the JVM alive or leave work in an
undefined state during application stop.

Spring-managed executors or lifecycle hooks are often a better fit in services.

## No Backpressure Or Rejection Policy

Accepting unlimited work is not resilience.

When the service is saturated, the executor should have a clear behavior:
reject, slow callers, run in caller thread, shed low-priority work, or fail fast.

## Blocking In The Wrong Pool

Mixing CPU-heavy work and blocking I/O in the same pool makes sizing difficult
and can cause unrelated workloads to starve each other.

Separate pools by workload when the behavior and limits are different.

## Missing Thread Names

Unnamed or generic pool threads make thread dumps and logs harder to interpret.

Use a thread factory with names tied to the workload.

## Key Idea

An executor is production infrastructure. Treat its limits, queue, naming,
shutdown, and failure policy as first-class design choices.
