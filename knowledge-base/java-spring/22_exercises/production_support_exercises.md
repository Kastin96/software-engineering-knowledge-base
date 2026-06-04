# Production Support Exercises

## Exercise 1: Actuator Setup

Configure actuator for a service.

Acceptance criteria:

- health and info are exposed;
- sensitive endpoints are not publicly exposed;
- health details follow environment rules;
- management exposure is documented.

## Exercise 2: Metrics

Add a custom metric for an important operation.

Acceptance criteria:

- metric name is stable;
- tags are low-cardinality;
- metric is useful for dashboarding or alerting.

## Exercise 3: Incident Investigation

Write a runbook for a slow endpoint.

Acceptance criteria:

- first checks are listed;
- relevant logs, metrics, and traces are named;
- rollback criteria are defined;
- follow-up actions are included.
