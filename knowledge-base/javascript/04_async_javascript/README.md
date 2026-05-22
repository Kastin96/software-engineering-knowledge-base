# Async JavaScript

This section explains how JavaScript handles work that finishes later.

After finishing it, you should understand why JavaScript can start async work,
continue running other code, and come back when a result is ready. You should
also be able to use callbacks, promises, `async` and `await`, timers, `fetch`,
and practical async error handling.

## Topics

- 01\. [Event Loop](event_loop.md)
- 02\. [Callbacks](callbacks.md)
- 03\. [Promises](promises.md)
- 04\. [Async and Await](async_await.md)
- 05\. [Timers](timers.md)
- 06\. [Fetch](fetch.md)
- 07\. [Async Error Handling](error_handling_async.md)

## Suggested Learning Flow

Start with the event loop to understand the mental model. Then learn callbacks
because many older APIs still use them. After that, study promises and
`async`/`await`, which are the main tools in modern JavaScript. Finish with
timers, `fetch`, and async error handling because they are common in real apps.

## Mini Goal

By the end of this section, try to write a small script that:

- fetches data from an API;
- shows a loading state before the request finishes;
- handles failed requests;
- runs two independent async operations in parallel;
- uses a timer to delay a retry or a UI update.

