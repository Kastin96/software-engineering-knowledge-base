# Timers

## Goal

Understand how to schedule code with `setTimeout` and `setInterval`.

## Why It Matters

Timers are used for delays, retries, polling, debouncing, throttling, UI
messages, and background checks.

## setTimeout

`setTimeout` runs a function after a delay.

```javascript
setTimeout(() => {
  console.log("Runs later");
}, 1000);
```

The delay is in milliseconds.

```javascript
const oneSecond = 1000;

setTimeout(() => {
  console.log("One second passed");
}, oneSecond);
```

## Clearing a Timeout

`clearTimeout` cancels a scheduled timeout.

```javascript
const timeoutId = setTimeout(() => {
  console.log("This will not run");
}, 1000);

clearTimeout(timeoutId);
```

## setInterval

`setInterval` runs a function repeatedly.

```javascript
const intervalId = setInterval(() => {
  console.log("Runs again and again");
}, 1000);

setTimeout(() => {
  clearInterval(intervalId);
}, 3500);
```

## Timer-Based Promise

Timers are often wrapped in promises.

```javascript
function wait(milliseconds) {
  return new Promise((resolve) => {
    setTimeout(resolve, milliseconds);
  });
}

async function run() {
  console.log("start");
  await wait(1000);
  console.log("after one second");
}

run();
```

## Simple Retry Delay

```javascript
function wait(milliseconds) {
  return new Promise((resolve) => {
    setTimeout(resolve, milliseconds);
  });
}

async function retryOnce(operation) {
  try {
    return await operation();
  } catch (error) {
    await wait(1000);
    return operation();
  }
}
```

## Real Pain Points

- Timer delays are minimum delays, not exact guarantees. Busy synchronous code
  can make timers run later.
- Forgetting to clear intervals can keep work running forever.
- `setTimeout(..., 0)` still waits until current synchronous code and promise
  microtasks finish.

```javascript
const intervalId = setInterval(() => {
  console.log("polling");
}, 1000);

// Stop polling when it is no longer needed.
clearInterval(intervalId);
```

## Practice

1. Run a function after two seconds with `setTimeout`.
2. Create an interval and stop it after five seconds.
3. Write a `wait` function that returns a promise.
4. Use `await wait(1000)` inside an async function.

## Related Topics

- [Event Loop](event_loop.md)
- [Promises](promises.md)
- [Async and Await](async_await.md)

