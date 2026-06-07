# Thread Pool Sizing

Thread pool sizing is a capacity decision, not a fixed formula.

The right size depends on whether work is CPU-bound, I/O-bound, or mostly
waiting on external resources.

## CPU-Bound Work

CPU-bound work spends most of its time computing.

Examples:

- data compression;
- encryption;
- CPU-heavy transformations;
- image or document processing.

For CPU-bound work, pool size is usually close to the number of available CPU
cores. Too many threads increase context switching without increasing throughput.

## I/O-Bound Work

I/O-bound work spends much of its time waiting.

Examples:

- database calls;
- HTTP calls;
- file operations;
- message broker operations.

I/O-bound pools can be larger than CPU-bound pools, but they must still respect
downstream limits such as connection pools, rate limits, and timeouts.

## Align With Downstream Resources

If an executor runs database work, it should not submit more concurrent database
calls than the connection pool and database can handle.

```text
executor concurrency > database pool size
```

This often creates waiting inside the application rather than useful throughput.

## Measure Under Load

Useful signals include:

- queue depth;
- task wait time;
- task execution time;
- rejection count;
- CPU usage;
- connection pool usage;
- downstream latency.

## Key Idea

Pool size should protect the service and its dependencies. Tune it with load
tests and production signals, not only with CPU count.
