# JVM, Memory, and GC

Spring Boot services run on the JVM, so production support sometimes requires
understanding memory and garbage collection behavior.

## What To Watch

Watch:

- heap usage;
- GC pause duration;
- allocation rate;
- thread count;
- loaded classes;
- direct memory;
- container memory limit;
- out-of-memory events.

## Memory Pressure Sources

Common sources:

- loading too much data into memory;
- unbounded caches;
- large request or response bodies;
- retaining objects in static collections;
- excessive buffering;
- high cardinality metrics;
- thread pools with too many threads.

## Heap Dump Rule

Heap dumps can contain sensitive data. Treat them as production secrets.

Do not casually expose actuator `heapdump` over public HTTP.

## JVM Options

Modern JVMs handle container memory better than old runtimes, but the service
still needs sane memory limits.

```text
-XX:MaxRAMPercentage=75
```

Use JVM options intentionally and document why they exist.

## Key Idea

Most Java memory incidents are not solved by immediately changing the garbage
collector. First find what is being allocated or retained.
