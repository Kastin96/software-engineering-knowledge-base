# Node.js Fundamentals Interview Questions

## What this document covers

This document covers Node.js fundamentals for backend and full-stack interviews. It focuses on practical concepts such as modules, npm, environment variables, middleware, and asynchronous behavior.

## Interview Questions

1. **What is Node.js?**

   Node.js is a runtime that allows JavaScript to run outside the browser. It is commonly used to build backend services, APIs, command-line tools, and real-time applications.

2. **Why use Node.js for backend development?**

   Node.js is fast for I/O-heavy applications, uses JavaScript on both frontend and backend, has a large npm ecosystem, and works well for APIs and real-time features.

3. **Is Node.js single-threaded?**

   JavaScript code in Node.js usually runs on a single main thread. However, Node.js can handle many async operations using the event loop and background system threads managed by its runtime.

4. **What is the event loop?**

   The event loop is the mechanism that lets Node.js handle asynchronous tasks. It runs synchronous code first, then processes callbacks, timers, promises, and I/O tasks when they are ready.

   ```javascript
   console.log("Start");

   setTimeout(() => {
     console.log("Timer finished");
   }, 0);

   console.log("End");
   ```

5. **What is non-blocking I/O?**

   Non-blocking I/O means Node.js can start an operation like reading a file or calling an API and continue running other code while waiting for the result.

6. **What is npm?**

   npm is the default package manager for Node.js. It is used to install libraries, manage dependencies, and run project scripts.

   ```bash
   npm install express
   npm run dev
   ```

7. **What is `package.json`?**

   `package.json` is a project configuration file. It stores project metadata, scripts, dependencies, and other settings.

   ```json
   {
     "name": "api-server",
     "version": "1.0.0",
     "scripts": {
       "start": "node src/server.js",
       "dev": "nodemon src/server.js"
     },
     "dependencies": {
       "express": "^4.18.2"
     },
     "devDependencies": {
       "nodemon": "^3.0.0"
     }
   }
   ```

8. **What is the difference between `dependencies` and `devDependencies`?**

   `dependencies` are packages needed to run the application in production. `devDependencies` are packages needed only during development, testing, or building.

9. **What is the difference between CommonJS and ES Modules?**

   CommonJS is the older Node.js module system that uses `require` and `module.exports`. ES Modules use `import` and `export`, and are the standard JavaScript module format.

10. **What is the difference between `require` and `import`?**

    `require` is used with CommonJS. `import` is used with ES Modules. Modern TypeScript and frontend code usually use `import`, while many older Node.js projects use `require`.

11. **What is middleware in Node.js/Express?**

    Middleware is a function that runs during the request-response cycle. It can read or modify the request, send a response, or pass control to the next middleware.

    ```javascript
    function logger(req, res, next) {
      console.log(`${req.method} ${req.url}`);
      next();
    }

    app.use(logger);
    ```

12. **What is `process.env`?**

    `process.env` is an object that contains environment variables available to the Node.js process. It is commonly used for configuration such as ports, database URLs, and API keys.

13. **How do you read environment variables?**

    Read values from `process.env`. Environment variables are strings, so convert them if you need numbers or booleans.

    ```javascript
    const port = process.env.PORT || 3000;

    app.listen(port, () => {
      console.log(`Server is running on port ${port}`);
    });
    ```

14. **What is `nodemon` or `ts-node-dev`?**

    `nodemon` restarts a Node.js app when files change. `ts-node-dev` does something similar for TypeScript projects, often running TypeScript directly during development.

    ```json
    {
      "scripts": {
        "dev": "nodemon src/server.js",
        "dev:ts": "ts-node-dev src/server.ts"
      }
    }
    ```

15. **What is JSON parsing in Express?**

    JSON parsing means converting a JSON request body into a JavaScript object. In Express, `express.json()` is commonly used for this.

    ```javascript
    const express = require("express");
    const app = express();

    app.use(express.json());

    app.post("/users", (req, res) => {
      console.log(req.body);
      res.status(201).json({ created: true });
    });
    ```

16. **How does Node.js handle async operations?**

    Node.js starts async operations, such as file reads or network calls, and continues running other code. When the result is ready, a callback, Promise, or async function handles it.

    ```javascript
    function fetchUserFromApi(userId) {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve({ id: userId, name: "Alex" });
        }, 500);
      });
    }

    async function printUser() {
      const user = await fetchUserFromApi(1);
      console.log(user);
    }

    printUser();
    ```

17. **What are error-first callbacks?**

    Error-first callbacks are a common Node.js pattern where the first callback argument is an error, and the second argument is the successful result.

    ```javascript
    const fs = require("fs");

    fs.readFile("data.txt", "utf8", (error, data) => {
      if (error) {
        console.log("Failed to read file:", error.message);
        return;
      }

      console.log(data);
    });
    ```

18. **What is a module?**

    A module is a file or package that contains reusable code. Modules help split an application into smaller, maintainable parts.

19. **How do you export and import functions?**

    In CommonJS, use `module.exports` and `require`. In ES Modules, use `export` and `import`.

    ```javascript
    // math.js - CommonJS
    function add(a, b) {
      return a + b;
    }

    module.exports = { add };
    ```

    ```javascript
    // app.js - CommonJS
    const { add } = require("./math");

    console.log(add(2, 3));
    ```

    ```javascript
    // math.js - ES Module
    export function add(a, b) {
      return a + b;
    }
    ```

    ```javascript
    // app.js - ES Module
    import { add } from "./math.js";

    console.log(add(2, 3));
    ```

20. **What are common Node.js interview mistakes?**

    Common mistakes include saying Node.js is only single-threaded without mentioning async runtime behavior, confusing npm with Node.js, not understanding `package.json`, forgetting to parse JSON request bodies, exposing secrets instead of using environment variables, and mixing CommonJS and ES Modules incorrectly.

    ```javascript
    // Mistake: req.body may be undefined without app.use(express.json())
    app.post("/users", (req, res) => {
      res.json(req.body);
    });
    ```
