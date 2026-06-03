# Common Mistakes

## Exposing Too Many Endpoints

`management.endpoints.web.exposure.include=*` is convenient locally, but it is a
bad production default.

Expose endpoints intentionally.

## Logging Secrets

Never log authorization headers, passwords, tokens, private keys, or full
request bodies by default.

This mistake is difficult to undo because logs are copied to external systems.

## Using Metrics As Logs

Metrics should have small stable tag sets. Do not put `orderId`, `userId`, or
`requestId` into metric tags.

## Health Checks That Are Too Heavy

Health checks should be fast. Do not perform expensive downstream calls on every
probe.

## Debug Logging Left Enabled

Debug logging can be useful during an incident, but leaving it enabled can
increase cost, hide important logs, and expose data.

## No Correlation Between Signals

Logs, metrics, and traces are strongest when they share service name,
environment, and trace context.

## Treating Actuator As Documentation

Actuator can show runtime state, but it should not replace deployment notes,
dashboards, runbooks, and alert descriptions.

## Key Idea

Observability should reduce production uncertainty. If the setup creates more
noise, risk, or exposed data, it needs to be simplified.
