# Performance Mindset

Performance work starts with a symptom, not with a random optimization.

Good questions:

- Which endpoint, job, or consumer is slow?
- Is the problem latency, throughput, memory, CPU, error rate, or saturation?
- Did the behavior change after a deployment or traffic change?
- Is the bottleneck inside the app, database, broker, or downstream service?

## Common Bottlenecks

In Spring backend services, common bottlenecks are:

- slow SQL queries;
- N+1 selects;
- missing database indexes;
- remote calls without timeouts;
- thread pool exhaustion;
- connection pool exhaustion;
- large response payloads;
- inefficient serialization;
- excessive logging;
- blocking calls inside reactive flows.

## Optimization Rule

Prefer this order:

1. Measure the problem.
2. Identify the bottleneck.
3. Change one thing.
4. Measure again.

Do not optimize code just because it looks inefficient. Optimize when the data
shows it matters.

## Key Idea

Production performance is usually about bottlenecks and saturation, not clever
code tricks.
