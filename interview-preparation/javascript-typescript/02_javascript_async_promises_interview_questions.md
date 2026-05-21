# JavaScript Async and Promises Interview Questions

## What this document covers

This document covers asynchronous JavaScript, Promises, `async/await`, API calls, and error handling. It is designed for beginner-to-mid-level Software Engineer and Full-Stack interviews.

## Interview Questions

1. **What is asynchronous programming?**

   Asynchronous programming lets JavaScript start a task and continue running other code while waiting for that task to finish. It is commonly used for API calls, timers, file operations, and database requests.

   ```javascript
   console.log("Start");

   setTimeout(() => {
     console.log("Timer finished");
   }, 1000);

   console.log("End");
   ```

2. **Why do we need async code in JavaScript?**

   We need async code because some operations take time. Without async code, the application could freeze while waiting for network requests, timers, or other slow tasks.

3. **What is a Promise?**

   A Promise is an object that represents a value that may be available now, later, or never. It is used to handle asynchronous operations.

   ```javascript
   const promise = new Promise((resolve, reject) => {
     const success = true;

     if (success) {
       resolve("Operation completed");
     } else {
       reject("Operation failed");
     }
   });

   promise.then((result) => {
     console.log(result);
   });
   ```

4. **What are the Promise states: `pending`, `fulfilled`, and `rejected`?**

   A Promise starts as `pending`. It becomes `fulfilled` when the operation succeeds. It becomes `rejected` when the operation fails.

5. **How does `.then()` work?**

   `.then()` runs when a Promise is fulfilled. It receives the resolved value and can return another value or another Promise.

   ```javascript
   Promise.resolve(10)
     .then((value) => value * 2)
     .then((value) => {
       console.log(value); // 20
     });
   ```

6. **How does `.catch()` work?**

   `.catch()` runs when a Promise is rejected. It is used to handle errors from the Promise chain.

   ```javascript
   Promise.reject("Something went wrong")
     .catch((error) => {
       console.log(error);
     });
   ```

7. **How does `.finally()` work?**

   `.finally()` runs after a Promise is settled, whether it was fulfilled or rejected. It is useful for cleanup tasks such as hiding a loading spinner.

   ```javascript
   Promise.resolve("Done")
     .then((result) => {
       console.log(result);
     })
     .finally(() => {
       console.log("Cleanup");
     });
   ```

8. **What is `async/await`?**

   `async/await` is a cleaner way to write Promise-based code. An `async` function returns a Promise, and `await` pauses inside that function until the Promise settles.

   ```javascript
   async function getMessage() {
     const message = await Promise.resolve("Hello");
     return message;
   }

   getMessage().then((message) => {
     console.log(message); // Hello
   });
   ```

9. **What is the difference between `Promise.then()` and `async/await`?**

   Both handle Promises. `.then()` uses chained callbacks, while `async/await` looks more like normal synchronous code and is often easier to read.

   ```javascript
   fetch("/api/users")
     .then((response) => response.json())
     .then((users) => console.log(users));

   async function loadUsers() {
     const response = await fetch("/api/users");
     const users = await response.json();
     console.log(users);
   }
   ```

10. **How do you handle errors with `async/await`?**

    Use `try/catch` around the code that may fail.

    ```javascript
    async function loadUser() {
      try {
        const response = await fetch("/api/user");
        const user = await response.json();
        console.log(user);
      } catch (error) {
        console.log("Failed to load user:", error);
      }
    }
    ```

11. **What happens if `await` is used without `try/catch`?**

    If the awaited Promise rejects and there is no `try/catch`, the async function returns a rejected Promise. The caller must handle it with `.catch()` or another `try/catch`.

    ```javascript
    async function loadData() {
      const result = await Promise.reject("Request failed");
      return result;
    }

    loadData().catch((error) => {
      console.log(error);
    });
    ```

12. **What is the difference between sequential and parallel async calls?**

    Sequential calls wait for one operation to finish before starting the next. Parallel calls start multiple operations at the same time and wait for all of them.

    ```javascript
    function wait(message, delay) {
      return new Promise((resolve) => {
        setTimeout(() => resolve(message), delay);
      });
    }

    async function runSequential() {
      const first = await wait("First", 1000);
      const second = await wait("Second", 1000);

      console.log(first, second);
    }

    async function runParallel() {
      const firstPromise = wait("First", 1000);
      const secondPromise = wait("Second", 1000);

      const first = await firstPromise;
      const second = await secondPromise;

      console.log(first, second);
    }
    ```

13. **What is `Promise.all`?**

    `Promise.all` runs multiple Promises in parallel and returns results when all of them fulfill. If one Promise rejects, the whole `Promise.all` rejects.

    ```javascript
    async function loadDashboard() {
      const [user, notifications] = await Promise.all([
        fetch("/api/user").then((response) => response.json()),
        fetch("/api/notifications").then((response) => response.json()),
      ]);

      console.log(user, notifications);
    }
    ```

14. **What is `Promise.allSettled`?**

    `Promise.allSettled` waits for all Promises to finish, even if some fail. It returns the status and result of each Promise.

    ```javascript
    const results = await Promise.allSettled([
      Promise.resolve("Success"),
      Promise.reject("Failure"),
    ]);

    console.log(results);
    ```

15. **What is `Promise.race`?**

    `Promise.race` returns the result of the first Promise that settles, whether it is fulfilled or rejected.

    ```javascript
    const result = await Promise.race([
      new Promise((resolve) => setTimeout(() => resolve("Fast"), 500)),
      new Promise((resolve) => setTimeout(() => resolve("Slow"), 1000)),
    ]);

    console.log(result); // Fast
    ```

16. **What is `Promise.any`?**

    `Promise.any` returns the first fulfilled Promise. It rejects only if all Promises reject.

    ```javascript
    const result = await Promise.any([
      Promise.reject("First failed"),
      Promise.resolve("First success"),
      Promise.resolve("Second success"),
    ]);

    console.log(result); // First success
    ```

17. **How do you call an API with `fetch`?**

    Use `fetch()` with a URL. It returns a Promise that resolves to a response object.

    ```javascript
    async function loadPosts() {
      const response = await fetch("/api/posts");
      const posts = await response.json();

      console.log(posts);
    }
    ```

18. **How do you process a JSON response?**

    Call `response.json()` to parse the response body as JSON. This also returns a Promise, so you usually use `await`.

    ```javascript
    async function loadUser() {
      const response = await fetch("/api/user");
      const user = await response.json();

      console.log(user.name);
    }
    ```

19. **How do you handle HTTP errors?**

    `fetch` only rejects for network errors. For HTTP errors like `404` or `500`, check `response.ok` and throw an error yourself.

    ```javascript
    async function loadProduct() {
      try {
        const response = await fetch("/api/products/1");

        if (!response.ok) {
          throw new Error(`HTTP error: ${response.status}`);
        }

        const product = await response.json();
        console.log(product);
      } catch (error) {
        console.log("Could not load product:", error.message);
      }
    }
    ```

20. **What are common async mistakes in interviews?**

    Common mistakes include forgetting to `await`, using `await` inside a loop when parallel execution is better, not handling errors, thinking `fetch` rejects on HTTP errors, and not understanding the output order of async code.

    ```javascript
    async function example() {
      const promise = Promise.resolve("Done");

      // Mistake: this logs the Promise object, not the resolved value
      console.log(promise);

      const result = await promise;
      console.log(result); // Done
    }
    ```
