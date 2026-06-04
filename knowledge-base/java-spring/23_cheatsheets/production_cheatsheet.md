# Production Cheatsheet

## Signals

- logs: events and context;
- metrics: rates, latency, saturation, errors;
- traces: request path across services;
- health: readiness and liveness;
- alerts: user impact and operational thresholds.

## Must-Haves

- timeouts for outbound calls;
- structured logs in shared environments;
- actuator health configured;
- sensitive actuator endpoints protected;
- low-cardinality metric tags;
- database and HTTP pool awareness;
- runbook for common incidents.

## Incident Checks

Check:

- recent deployment;
- error rate;
- p95/p99 latency;
- database query duration;
- downstream call failures;
- thread and connection pool usage;
- Kafka lag or DLT count if messaging is involved.

## Watch For

- debug logging left enabled;
- `include=*` actuator exposure;
- metrics with user id or request id tags;
- missing rollback criteria.
