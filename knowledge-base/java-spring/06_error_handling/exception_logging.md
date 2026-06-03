# Exception Logging

Exception logging should help diagnose incidents without duplicating noise or
leaking sensitive data.

## Log At The Right Level

Not every exception is an application error.

```text
validation failure     -> usually not ERROR
not found              -> often not ERROR
business conflict      -> usually WARN or no log, depending on context
downstream failure     -> WARN or ERROR
unexpected bug         -> ERROR
```

If every invalid request logs at `ERROR`, real production issues become harder
to find.

## Avoid Double Logging

If a service logs an exception and the global handler logs it again, the same
failure appears multiple times. Prefer logging at the boundary where you have
request context and can choose the correct severity.

## Useful Context

Useful log context may include:

- request ID or trace ID;
- user or tenant identifier when safe;
- resource ID;
- operation name;
- downstream dependency name;
- sanitized error code.

## Sensitive Data

Do not log:

- passwords or tokens;
- full authorization headers;
- private keys;
- full payment data;
- large request bodies by default.

## Key Idea

Log exceptions for operators, not for clients. Keep logs useful, deduplicated,
and safe.
