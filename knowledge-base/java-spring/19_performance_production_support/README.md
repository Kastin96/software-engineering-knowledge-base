# Performance and Production Support

This section covers performance thinking and production support for Spring Boot
applications.

The focus is not micro-optimizing code. The focus is finding bottlenecks,
configuring the runtime sensibly, and keeping services operable under real load.

## Topics

- 01\. [Performance Mindset](performance_mindset.md)
- 02\. [Measuring Before Optimizing](measuring_before_optimizing.md)
- 03\. [Database Performance](database_performance.md)
- 04\. [HTTP Clients and Timeouts](http_clients_timeouts.md)
- 05\. [Connection Pools and Threads](connection_pools_threads.md)
- 06\. [Caching](caching.md)
- 07\. [JVM, Memory, and GC](jvm_memory_gc.md)
- 08\. [Resilience and Degradation](resilience_degradation.md)
- 09\. [Production Support Runbook](production_support_runbook.md)
- 10\. [Common Mistakes](common_mistakes.md)

## Suggested Learning Flow

Start with measurement and database performance. Most backend bottlenecks come
from queries, remote calls, thread starvation, or missing timeouts. Then move to
caching, JVM behavior, resilience, and support workflows.

## Mini Goal

By the end of this section, you should be able to:

- explain why profiling and metrics come before optimization;
- identify common Spring service bottlenecks;
- configure timeouts for outbound calls;
- reason about database indexes and query count;
- use caching where it improves latency without breaking correctness;
- understand pool sizing at a practical level;
- diagnose production issues using logs, metrics, traces, and actuator.

## Interview Readiness

You should be able to answer:

- How would you investigate a slow Spring Boot endpoint?
- What is an N+1 query problem?
- Why are missing timeouts dangerous?
- When is caching useful and when is it risky?
- How can connection pools cause production incidents?
- What metrics would you check during an incident?
- What would you include in a production runbook?
