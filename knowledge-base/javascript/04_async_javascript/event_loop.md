# Event Loop

## Goal

Understand how JavaScript runs synchronous code and schedules asynchronous work.

## Why It Matters

The event loop explains why `setTimeout`, promises, user events, network
requests, and rendering do not always run in the order beginners expect.

## Explanation

JavaScript runs code on a call stack. Synchronous code runs first, one statement
at a time.

```javascript
console.log("first");
console.log("second");
console.log("third");
```

Output:

```text
first
second
third
```

Async APIs let JavaScript start work that finishes later. When that work is
ready, its callback or promise reaction is scheduled to run after the current
synchronous code finishes.

```javascript
console.log("start");

setTimeout(() => {
  console.log("timer");
}, 0);

console.log("end");
```

Output:

```text
start
end
timer
```

The timer waits until the current call stack is empty.

## Tasks and Microtasks

Promise callbacks run as microtasks. Timer callbacks run as tasks. Microtasks
are processed before the next task.

```javascript
console.log("start");

setTimeout(() => {
  console.log("timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("promise");
});

console.log("end");
```

Output:

```text
start
end
promise
timeout
```

## Async Functions and the Event Loop

`await` pauses the async function, but it does not block the whole program.

```javascript
async function run() {
  console.log("inside before await");
  await Promise.resolve();
  console.log("inside after await");
}

console.log("start");
run();
console.log("end");
```

Output:

```text
start
inside before await
end
inside after await
```

## Practical Mental Model

Think about async code in this order:

1. Synchronous code runs first.
2. Promise reactions continue after the current stack.
3. Timers and other queued tasks run later.
4. Async work does not make CPU-heavy code faster by itself.

## Real Pain Points

- A timer with `0` milliseconds does not run immediately. It runs after current
  synchronous code and promise microtasks.
- `await` pauses only the current async function, not the whole JavaScript
  runtime.
- Long synchronous work still blocks the thread.

```javascript
console.log("start");

setTimeout(() => {
  console.log("timer waits for the loop");
}, 0);

for (let index = 0; index < 1_000_000_000; index += 1) {
  // Long synchronous work blocks later callbacks.
}

console.log("end");
```

## Practice

1. Predict the output order for code with `console.log`, `setTimeout`, and a promise.
2. Write an async function with one `await` and explain which lines run first.
3. Create a long synchronous loop and observe that timers wait.

## Related Topics

- [Promises](promises.md)
- [Async and Await](async_await.md)
- [Timers](timers.md)

