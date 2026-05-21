# Express.js REST API Interview Questions

## What this document covers

This document covers Express.js REST API development for full-stack and backend interviews. It focuses on routes, request handling, middleware, validation, errors, and practical API structure.

## Interview Questions

1. **What is Express.js?**

   Express.js is a lightweight Node.js framework for building web servers and APIs. It provides routing, middleware support, and helpers for handling HTTP requests and responses.

2. **How do you create an Express app?**

   Create an app with `express()`, add middleware and routes, then start the server with `app.listen()`.

   ```javascript
   const express = require("express");

   const app = express();
   const port = process.env.PORT || 3000;

   app.use(express.json());

   app.listen(port, () => {
     console.log(`Server is running on port ${port}`);
   });
   ```

3. **What does `express.json()` do?**

   `express.json()` parses incoming JSON request bodies and makes the parsed data available as `req.body`.

   ```javascript
   app.use(express.json());
   ```

4. **What is a route?**

   A route defines how the server responds to a specific HTTP method and path, such as `GET /api/users`.

   ```javascript
   app.get("/api/users", (req, res) => {
     res.json([]);
   });
   ```

5. **What is `req`?**

   `req` is the request object. It contains information from the client, such as route parameters, query parameters, headers, and body data.

6. **What is `res`?**

   `res` is the response object. It is used to send data, set status codes, and finish the HTTP response.

   ```javascript
   res.status(200).json({ message: "Success" });
   ```

7. **What is `next`?**

   `next` is a function that passes control to the next middleware or error handler.

   ```javascript
   function logger(req, res, next) {
     console.log(`${req.method} ${req.url}`);
     next();
   }
   ```

8. **How do you create a GET endpoint?**

   Use `app.get()` with a path and handler function.

   ```javascript
   const users = [
     { id: 1, name: "Alex" },
     { id: 2, name: "Example User" },
   ];

   app.get("/api/users", (req, res) => {
     res.json(users);
   });
   ```

9. **How do you create a POST endpoint?**

   Use `app.post()` and read the request body from `req.body`.

   ```javascript
   app.post("/api/users", (req, res) => {
     const { name } = req.body;

     const user = {
       id: users.length + 1,
       name,
     };

     users.push(user);
     res.status(201).json(user);
   });
   ```

10. **How do you read `req.params`?**

    `req.params` contains values from dynamic route segments, such as `:id`.

    ```javascript
    app.get("/api/users/:id", (req, res) => {
      const id = Number(req.params.id);
      const user = users.find((item) => item.id === id);

      if (!user) {
        return res.status(404).json({ message: "User not found" });
      }

      res.json(user);
    });
    ```

11. **How do you read `req.query`?**

    `req.query` contains query string values from the URL, such as `/api/users?active=true`.

    ```javascript
    app.get("/api/users", (req, res) => {
      const { active } = req.query;

      if (active === "true") {
        return res.json(users.filter((user) => user.active));
      }

      res.json(users);
    });
    ```

12. **How do you read `req.body`?**

    Use `express.json()` first, then read JSON fields from `req.body`.

    ```javascript
    app.use(express.json());

    app.post("/api/users", (req, res) => {
      const { name, email } = req.body;
      res.status(201).json({ name, email });
    });
    ```

13. **How do you return JSON?**

    Use `res.json()` to send a JSON response.

    ```javascript
    res.json({ status: "ok" });
    ```

14. **How do you set an HTTP status code?**

    Use `res.status(code)` before sending the response.

    ```javascript
    res.status(201).json({ created: true });
    ```

15. **What is middleware?**

    Middleware is a function that runs during the request-response cycle. It can log requests, parse data, check authentication, validate input, or handle errors.

    ```javascript
    function requestLogger(req, res, next) {
      console.log(`${req.method} ${req.path}`);
      next();
    }

    app.use(requestLogger);
    ```

16. **What is the difference between application middleware and route middleware?**

    Application middleware runs for many or all routes using `app.use()`. Route middleware runs only for specific routes.

    ```javascript
    app.use(requestLogger);

    function requireAuth(req, res, next) {
      const token = req.headers.authorization;

      if (!token) {
        return res.status(401).json({ message: "Unauthorized" });
      }

      next();
    }

    app.get("/api/profile", requireAuth, (req, res) => {
      res.json({ name: "Alex" });
    });
    ```

17. **What is error-handling middleware?**

    Error-handling middleware catches errors and returns a consistent error response. It has four parameters: `err`, `req`, `res`, and `next`.

    ```javascript
    function errorHandler(err, req, res, next) {
      const statusCode = err.statusCode || 500;

      res.status(statusCode).json({
        message: err.message || "Internal server error",
      });
    }

    app.use(errorHandler);
    ```

18. **How do you validate request body?**

    Check required fields before creating or updating data. In real projects, validation libraries like Zod, Joi, or express-validator are often used.

    ```javascript
    app.post("/api/users", (req, res) => {
      const { name, email } = req.body;

      if (!name || !email) {
        return res.status(400).json({ message: "Name and email are required" });
      }

      const user = { id: users.length + 1, name, email };
      users.push(user);

      res.status(201).json(user);
    });
    ```

19. **How do you return `400`, `404`, and `500` errors?**

    Return `400` for invalid client input, `404` when a resource is not found, and `500` for unexpected server errors.

    ```javascript
    app.get("/api/users/:id", (req, res) => {
      const id = Number(req.params.id);

      if (Number.isNaN(id)) {
        return res.status(400).json({ message: "Invalid user id" });
      }

      const user = users.find((item) => item.id === id);

      if (!user) {
        return res.status(404).json({ message: "User not found" });
      }

      res.json(user);
    });
    ```

20. **How should you structure an Express app with routes, controllers, services, and repositories?**

    Routes define URLs. Controllers handle HTTP request and response logic. Services contain business logic. Repositories handle database access.

    ```text
    src/
      app.js
      routes/
        userRoutes.js
      controllers/
        userController.js
      services/
        userService.js
      repositories/
        userRepository.js
    ```

21. **What is the difference between `PUT` and `PATCH`?**

    `PUT` usually replaces a full resource. `PATCH` updates only selected fields.

    ```javascript
    app.patch("/api/users/:id", (req, res) => {
      const id = Number(req.params.id);
      const user = users.find((item) => item.id === id);

      if (!user) {
        return res.status(404).json({ message: "User not found" });
      }

      Object.assign(user, req.body);
      res.json(user);
    });
    ```

22. **How do you implement a health endpoint?**

    A health endpoint is a simple route that confirms the API is running.

    ```javascript
    app.get("/health", (req, res) => {
      res.status(200).json({ status: "ok" });
    });
    ```

23. **What are common Express mistakes?**

    Common mistakes include forgetting `express.json()`, not returning after sending an error response, mixing business logic into routes, not using centralized error handling, not validating request bodies, and exposing internal error details to clients.

    ```javascript
    app.post("/api/users", (req, res) => {
      if (!req.body.email) {
        res.status(400).json({ message: "Email is required" });
        return;
      }

      res.status(201).json({ id: 1, email: req.body.email });
    });
    ```

## Compact CRUD Example

This example shows a small Express API with the endpoints often expected in live coding.

```javascript
const express = require("express");

const app = express();
const port = process.env.PORT || 3000;

app.use(express.json());

let users = [
  { id: 1, name: "Alex", email: "user@example.test", active: true },
  { id: 2, name: "Example User", email: "secondary.user@example.test", active: true },
];

app.get("/health", (req, res) => {
  res.status(200).json({ status: "ok" });
});

app.get("/api/users", (req, res) => {
  res.json(users);
});

app.get("/api/users/:id", (req, res) => {
  const id = Number(req.params.id);
  const user = users.find((item) => item.id === id);

  if (!user) {
    return res.status(404).json({ message: "User not found" });
  }

  res.json(user);
});

app.post("/api/users", (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ message: "Name and email are required" });
  }

  const user = {
    id: users.length + 1,
    name,
    email,
    active: true,
  };

  users.push(user);
  res.status(201).json(user);
});

app.patch("/api/users/:id", (req, res) => {
  const id = Number(req.params.id);
  const user = users.find((item) => item.id === id);

  if (!user) {
    return res.status(404).json({ message: "User not found" });
  }

  Object.assign(user, req.body);
  res.json(user);
});

app.delete("/api/users/:id", (req, res) => {
  const id = Number(req.params.id);
  const userExists = users.some((item) => item.id === id);

  if (!userExists) {
    return res.status(404).json({ message: "User not found" });
  }

  users = users.filter((item) => item.id !== id);
  res.status(204).send();
});

app.use((err, req, res, next) => {
  console.error(err);

  res.status(500).json({
    message: "Internal server error",
  });
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```
