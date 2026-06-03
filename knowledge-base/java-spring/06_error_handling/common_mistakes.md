# Common Mistakes

Error handling mistakes usually create inconsistent client behavior or noisy
production diagnostics.

## Catching Exceptions In Every Controller

Manual `try/catch` blocks in each endpoint duplicate mapping logic and make API
errors inconsistent. Use a global handler for common failures.

## Returning Raw Exception Messages

Raw messages can expose implementation details and are often unstable. Use
client-safe messages and stable error codes.

## Treating All Client Errors As `500`

Validation failures, not found resources, and state conflicts are not internal
server errors. Map them intentionally.

## Logging Every Failure At `ERROR`

Expected client-side failures should not flood production logs as server
incidents.

## Exposing Infrastructure Details

Do not return SQL errors, table names, stack traces, internal URLs, or raw
downstream payloads to clients.

## No Stable Error Codes

If clients need to branch on error behavior, stable error codes are safer than
parsing human-readable messages.

## Key Idea

Good error handling gives clients a stable contract and gives operators enough
context to diagnose failures.
