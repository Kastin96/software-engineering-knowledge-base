# Quick Cheat Sheet Before Full-Stack Assessment

## 1. JavaScript Must-Know Concepts

- Prefer `const` by default, use `let` when reassignment is needed, avoid `var`.
- Use `===` instead of `==`.
- Falsy values: `false`, `0`, `""`, `null`, `undefined`, `NaN`.
- Arrays: `map`, `filter`, `reduce`, `find`, `some`, `every`.
- Objects and arrays are reference types.
- Use spread for shallow copies.

```javascript
const updatedUser = { ...user, name: "Alex" };
const activeUsers = users.filter((user) => user.active);
```

### Async/Await

```javascript
async function loadUsers() {
  try {
    const response = await fetch("/api/users");
    const users = await response.json();
    return users;
  } catch (error) {
    console.log("Failed to load users", error);
    return [];
  }
}
```

## 2. TypeScript Must-Know Concepts

- TypeScript adds static typing to JavaScript.
- Use interfaces or types to describe object shapes.
- Use union types for multiple possible values.
- Use `unknown` instead of `any` when the value is not known.
- Use `Partial<T>` for update payloads.
- Use `Promise<T>` for async return types.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  active: boolean;
}

interface CreateUserRequest {
  name: string;
  email: string;
}

type UpdateUserRequest = Partial<CreateUserRequest>;

async function getUsers(): Promise<User[]> {
  return [];
}
```

## 3. Express.js Must-Know Syntax

- `express.json()` parses JSON request bodies.
- `req.params` reads path variables.
- `req.query` reads query string values.
- `req.body` reads JSON body data.
- `res.status(code).json(data)` sends JSON with status.

```javascript
const express = require("express");

const app = express();
app.use(express.json());

app.get("/health", (req, res) => {
  res.status(200).json({ status: "ok" });
});

app.post("/api/users", (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ message: "Name and email are required" });
  }

  res.status(201).json({ id: 1, name, email });
});
```

## 4. REST API Status Codes

- `200 OK`: request succeeded.
- `201 Created`: resource created.
- `204 No Content`: success with no response body.
- `400 Bad Request`: invalid input.
- `401 Unauthorized`: not authenticated.
- `403 Forbidden`: authenticated but not allowed.
- `404 Not Found`: resource does not exist.
- `500 Internal Server Error`: unexpected server error.

```json
{
  "message": "User not found",
  "statusCode": 404,
  "code": "USER_NOT_FOUND"
}
```

## 5. React Must-Know Hooks

- `useState`: stores component state.
- `useEffect`: runs side effects such as API calls.
- Props pass data from parent to child.
- Controlled inputs store form values in state.
- Render lists with `map()` and stable `key`.

```jsx
import { useEffect, useState } from "react";

function UsersPage() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function loadUsers() {
      const response = await fetch("/api/users");
      const data = await response.json();
      setUsers(data);
    }

    loadUsers();
  }, []);

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

## 6. SQL Must-Know Queries

- `SELECT` reads data.
- `WHERE` filters rows.
- `INSERT` creates rows.
- `UPDATE` changes rows.
- `DELETE` removes rows.
- `JOIN` combines related tables.

```sql
SELECT id, name, email
FROM users
WHERE id = 1;
```

```sql
SELECT users.name, orders.total_amount
FROM users
INNER JOIN orders ON orders.user_id = users.id;
```

## 7. Debugging Checklist

- Reproduce the issue.
- Read the error message carefully.
- Check browser Console and Network tab.
- Verify request URL, method, headers, and body.
- Check backend logs and stack trace.
- Check database query and data.
- Check environment variables.
- Check recent code changes.
- Add a small test or log to confirm the fix.

## 8. Common Interview Phrases

- "I would first reproduce the issue and identify whether it is frontend, backend, or data-related."
- "I would validate input before reaching the business logic."
- "I would keep controllers thin and put business rules in the service layer."
- "I would return consistent error responses with the correct HTTP status code."
- "I would use TypeScript types or interfaces to make API contracts clearer."
- "I would check the Network tab to confirm the request and response."
- "I would avoid mutating React state directly."

## 9. What to Say if Node.js Is Not Your Primary Stack

- Be honest, but connect your backend experience to Node.js concepts.
- Emphasize transferable skills: REST, validation, service layers, error handling, SQL, testing, and debugging.
- Mention that Express structure is similar to Spring Boot layers.

Example:

```text
Node.js is not my primary production stack, but I understand the backend concepts behind it. I can structure an Express API with routes, controllers, services, repositories, validation, and centralized error handling, similar to how I would structure a Spring Boot service.
```

## How to Explain My Background Honestly

My strongest background is backend engineering with Java and Spring Boot, but I have hands-on exposure to TypeScript, React, and Node.js/Express.js. I understand REST API design, validation, error handling, service-layer structure, and database integration, and I can apply the same backend principles in Node.js.
