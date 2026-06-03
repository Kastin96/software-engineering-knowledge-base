# Common Mistakes

## Optimizing Without Measurements

Changing code without metrics or profiling often moves the problem or hides it.

## Missing Timeouts

Outbound calls without timeouts can exhaust request threads and make unrelated
endpoints fail.

## Huge Database Pools

Increasing pool size can overload the database. Fix slow queries and right-size
the pool instead.

## Caching Without Invalidation

Caching stale data can create worse business bugs than slow reads.

## Ignoring P99 Latency

Average latency hides tail behavior. Production users often feel p95 and p99
more than averages.

## Excessive Logging

High-volume debug logs increase cost, add noise, and can slow the service.

## Treating Actuator As Public API

Production actuator endpoints are operational tools. Expose only what is needed
and secure sensitive endpoints.

## No Runbook

If every incident starts from scratch, the team learns too slowly.

## Key Idea

Performance and support mistakes usually come from missing boundaries: no
timeouts, no limits, no measurements, no ownership.
