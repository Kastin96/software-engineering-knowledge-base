# Common Mistakes

Raw threads are simple to create and easy to misuse.

## Creating One Thread Per Request

Creating platform threads per incoming request or per small task can exhaust
memory and damage latency under load.

Use application-server request handling, executors, virtual threads, or reactive
pipelines depending on the workload.

## Calling run Instead Of start

`run()` executes on the current thread. `start()` creates a new execution path.

This mistake removes concurrency while making the code look concurrent.

## Ignoring Shutdown

A manually started thread without a shutdown path can keep the JVM alive or
abandon work unpredictably.

Every long-running worker needs a stop signal and bounded shutdown behavior.

## Swallowing InterruptedException

Ignoring interruption breaks cooperative cancellation and can make application
shutdown slow or unreliable.

Restore the interrupt flag and exit the current operation when appropriate.

## Sharing Mutable State Casually

Threads sharing mutable state need clear synchronization, atomic variables,
immutable data, message passing, or another safe coordination strategy.

Visibility bugs can be intermittent and difficult to reproduce.

## Hiding Failures

Background thread failures should be logged, monitored, or surfaced to a
supervising component.

Silent thread death can leave a service running while critical work has stopped.

## Key Idea

Raw threads expose all lifecycle, safety, and failure concerns directly. Use
them when that control is necessary; otherwise move up to a managed abstraction.
