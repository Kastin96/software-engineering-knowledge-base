# Production Support Runbook

A runbook describes how to investigate and respond to known production
conditions.

It should be practical enough for the engineer on support duty to use under
pressure.

## Slow Endpoint Checklist

Check:

- recent deployments;
- request rate and error rate;
- p95 and p99 latency;
- database query duration;
- downstream call duration;
- connection pool usage;
- thread dumps;
- logs for correlation ids and errors.

## Consumer Lag Checklist

Check:

- consumer group assignments;
- lag per partition;
- processing duration;
- retry and DLT count;
- downstream dependency health;
- recent schema or payload changes.

## High Error Rate Checklist

Check:

- error type and first occurrence time;
- affected endpoint or consumer;
- deployment version;
- dependency status;
- configuration changes;
- whether rollback is safer than patching forward.

## Incident Notes

Record:

- impact;
- timeline;
- hypothesis;
- actions taken;
- rollback or mitigation;
- follow-up tasks.

## Key Idea

Good production support is repeatable. A runbook turns past incidents into
faster future diagnosis.
