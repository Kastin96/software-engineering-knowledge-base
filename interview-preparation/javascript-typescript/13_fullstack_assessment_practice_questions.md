# Full-Stack Assessment Practice Questions

## What this document covers

This document provides practice questions for a Software Engineer / Full-Stack online assessment. It focuses on JavaScript, TypeScript, Node.js, Express, React, REST APIs, SQL, debugging, and short coding tasks.

## 1. JavaScript Multiple-Choice Questions

1. **What is the output?**

   ```javascript
   console.log(typeof null);
   ```

   A. `"null"`  
   B. `"object"`  
   C. `"undefined"`  
   D. `"boolean"`

   **Answer:** B. `"object"`

   **Explanation:** This is a known JavaScript behavior. `null` means an intentional empty value, but `typeof null` returns `"object"`.

2. **Which value is falsy?**

   A. `"false"`  
   B. `[]`  
   C. `{}`  
   D. `0`

   **Answer:** D. `0`

   **Explanation:** `0`, `""`, `false`, `null`, `undefined`, and `NaN` are falsy. Empty arrays and objects are truthy.

3. **What is the difference between `==` and `===`?**

   A. No difference  
   B. `==` compares type only  
   C. `===` compares value and type  
   D. `===` converts types first

   **Answer:** C. `===` compares value and type.

   **Explanation:** Prefer `===` because it avoids unexpected type conversion.

4. **What is the output?**

   ```javascript
   console.log(2 + "2");
   ```

   A. `4`  
   B. `"22"`  
   C. `NaN`  
   D. Error

   **Answer:** B. `"22"`

   **Explanation:** JavaScript converts the number to a string and performs string concatenation.

5. **What does `map()` return?**

   A. The same array  
   B. A new transformed array  
   C. A single value  
   D. Nothing

   **Answer:** B. A new transformed array.

   ```javascript
   const numbers = [1, 2, 3];
   const doubled = numbers.map((number) => number * 2);
   // [2, 4, 6]
   ```

6. **What does `filter()` return?**

   A. A new array with matching items  
   B. The first matching item  
   C. A boolean  
   D. A number

   **Answer:** A. A new array with matching items.

   ```javascript
   const even = [1, 2, 3, 4].filter((number) => number % 2 === 0);
   // [2, 4]
   ```

7. **What does `reduce()` commonly do?**

   A. Removes items  
   B. Combines array values into one result  
   C. Sorts an array  
   D. Finds one item

   **Answer:** B. Combines array values into one result.

   ```javascript
   const total = [1, 2, 3].reduce((sum, number) => sum + number, 0);
   // 6
   ```

8. **What is a closure?**

   A. A function that remembers variables from its outer scope  
   B. A function that never returns  
   C. A loop inside another loop  
   D. A browser API

   **Answer:** A. A function that remembers variables from its outer scope.

   ```javascript
   function createCounter() {
     let count = 0;
     return () => count += 1;
   }
   ```

9. **What is the output?**

   ```javascript
   console.log(Boolean(""));
   ```

   A. `true`  
   B. `false`  
   C. `undefined`  
   D. Error

   **Answer:** B. `false`

   **Explanation:** An empty string is falsy.

10. **Which keyword should usually be used for a value that will not be reassigned?**

    A. `var`  
    B. `let`  
    C. `const`  
    D. `static`

    **Answer:** C. `const`

    **Explanation:** `const` prevents reassignment and communicates intent.

11. **What does optional chaining do?**

    A. Converts values to strings  
    B. Safely reads nested properties  
    C. Creates a Promise  
    D. Sorts arrays

    **Answer:** B. Safely reads nested properties.

    ```javascript
    const email = user.profile?.email;
    ```

12. **What does `??` check for?**

    A. Only `false`  
    B. Only `0`  
    C. `null` or `undefined`  
    D. Any falsy value

    **Answer:** C. `null` or `undefined`

    ```javascript
    const name = inputName ?? "Guest";
    ```

13. **What is the output order?**

    ```javascript
    console.log("A");
    setTimeout(() => console.log("B"), 0);
    console.log("C");
    ```

    A. A B C  
    B. B A C  
    C. A C B  
    D. C B A

    **Answer:** C. A C B

    **Explanation:** Synchronous code runs first. The timer callback runs later.

14. **What does `find()` return?**

    A. All matching items  
    B. The first matching item  
    C. A boolean  
    D. A new sorted array

    **Answer:** B. The first matching item.

15. **What is the output?**

    ```javascript
    const user = { name: "Alex" };
    const copy = user;
    copy.name = "Example User";
    console.log(user.name);
    ```

    A. `"Alex"`  
    B. `"Example User"`  
    C. `undefined`  
    D. Error

    **Answer:** B. `"Example User"`

    **Explanation:** Objects are reference types. `copy` and `user` point to the same object.

## 2. TypeScript Multiple-Choice Questions

1. **What is TypeScript?**

   A. A database  
   B. A typed superset of JavaScript  
   C. A CSS framework  
   D. A browser

   **Answer:** B. A typed superset of JavaScript.

   **Explanation:** TypeScript adds static typing and compiles to JavaScript.

2. **What does this type mean?**

   ```typescript
   let id: string | number;
   ```

   A. `id` must be both string and number  
   B. `id` can be string or number  
   C. `id` is optional  
   D. `id` is an array

   **Answer:** B. `id` can be string or number.

3. **What does `email?: string` mean?**

   A. `email` is required  
   B. `email` is optional  
   C. `email` must be null  
   D. `email` is a number

   **Answer:** B. `email` is optional.

4. **Which is safer than `any` for unknown input?**

   A. `never`  
   B. `unknown`  
   C. `void`  
   D. `object`

   **Answer:** B. `unknown`

   **Explanation:** `unknown` requires type checking before use.

5. **What does `Promise<User[]>` mean?**

   A. A user array returned immediately  
   B. A Promise that resolves to an array of users  
   C. A user object  
   D. A rejected Promise only

   **Answer:** B. A Promise that resolves to an array of users.

6. **What does `Partial<User>` do?**

   A. Makes all properties required  
   B. Makes all properties optional  
   C. Removes all properties  
   D. Converts `User` to string

   **Answer:** B. Makes all properties optional.

7. **Which keyword is often used to describe an object shape?**

   A. `interface`  
   B. `console`  
   C. `await`  
   D. `module`

   **Answer:** A. `interface`

   ```typescript
   interface User {
     id: number;
     email: string;
   }
   ```

8. **What does `Omit<User, "password">` do?**

   A. Keeps only `password`  
   B. Removes `password` from the type  
   C. Makes `password` optional  
   D. Converts `password` to number

   **Answer:** B. Removes `password` from the type.

9. **What is type inference?**

   A. TypeScript guesses a type from the value  
   B. TypeScript ignores all types  
   C. JavaScript validates SQL  
   D. React creates types automatically

   **Answer:** A. TypeScript guesses a type from the value.

10. **What is wrong with this code?**

    ```typescript
    function add(a: number, b: number): number {
      return "result";
    }
    ```

    **Answer:** The function promises to return a `number`, but it returns a `string`.

    **Explanation:** TypeScript catches return type mismatches before runtime.

## 3. Node.js and Express Questions

1. **What is Node.js?**

   **Answer:** Node.js is a runtime that lets JavaScript run outside the browser.

   **Explanation:** It is commonly used for backend APIs and server-side tools.

2. **What is Express.js?**

   **Answer:** Express is a Node.js framework for building web servers and APIs.

3. **What does `express.json()` do?**

   **Answer:** It parses incoming JSON request bodies and makes the result available as `req.body`.

4. **What is middleware?**

   **Answer:** Middleware is a function that runs during the request-response cycle.

   ```javascript
   app.use((req, res, next) => {
     console.log(req.method, req.path);
     next();
   });
   ```

5. **What is `req.params` used for?**

   **Answer:** It reads path parameters from the URL.

   ```javascript
   app.get("/api/users/:id", (req, res) => {
     res.json({ id: req.params.id });
   });
   ```

6. **What is `req.query` used for?**

   **Answer:** It reads query string values such as `?page=1`.

7. **What is `req.body` used for?**

   **Answer:** It reads data sent by the client, usually in POST or PATCH requests.

8. **How do you return JSON in Express?**

   **Answer:** Use `res.json()`.

   ```javascript
   res.status(200).json({ status: "ok" });
   ```

9. **How do you handle errors centrally in Express?**

   **Answer:** Add error-handling middleware with `err`, `req`, `res`, and `next`.

   ```javascript
   app.use((err, req, res, next) => {
     res.status(500).json({ message: "Internal server error" });
   });
   ```

10. **Why should controllers call services instead of containing all logic?**

    **Answer:** It keeps HTTP logic separate from business logic, making code easier to test and maintain.

## 4. React Questions

1. **What is React?**

   **Answer:** React is a JavaScript library for building user interfaces with components.

2. **What is a component?**

   **Answer:** A component is a reusable piece of UI.

   ```jsx
   function Greeting() {
     return <h1>Hello</h1>;
   }
   ```

3. **What are props?**

   **Answer:** Props are data passed from a parent component to a child component.

4. **What is state?**

   **Answer:** State is component data that can change and cause re-rendering.

5. **What does `useState` do?**

   **Answer:** It creates state in a functional component.

   ```jsx
   const [count, setCount] = useState(0);
   ```

6. **What does `useEffect` do?**

   **Answer:** It runs side effects such as API calls after render.

7. **How do you render a list in React?**

   **Answer:** Use `map()` and give each item a stable `key`.

   ```jsx
   users.map((user) => <li key={user.id}>{user.name}</li>);
   ```

8. **What is a controlled input?**

   **Answer:** A controlled input gets its value from React state and updates state on change.

   ```jsx
   <input value={name} onChange={(event) => setName(event.target.value)} />
   ```

9. **What causes a React component to re-render?**

   **Answer:** State changes, new props, context changes, or a parent re-render can cause re-rendering.

10. **What is conditional rendering?**

    **Answer:** It means showing different UI based on a condition.

    ```jsx
    {isLoading ? <p>Loading...</p> : <UserList users={users} />}
    ```

## 5. REST API Questions

1. **What is REST?**

   **Answer:** REST is an API style that uses HTTP methods and resource-based URLs.

2. **What is a resource?**

   **Answer:** A resource is an entity managed by the API, such as `users` or `orders`.

3. **What should `GET /api/users` do?**

   **Answer:** It should return a list of users.

4. **What should `GET /api/users/:id` do?**

   **Answer:** It should return one user by ID or `404` if the user does not exist.

5. **What should `POST /api/users` do?**

   **Answer:** It should create a new user and usually return `201 Created`.

6. **What is the difference between `PUT` and `PATCH`?**

   **Answer:** `PUT` replaces a full resource. `PATCH` updates selected fields.

7. **What does `400 Bad Request` mean?**

   **Answer:** The client sent invalid data.

8. **What does `401 Unauthorized` mean?**

   **Answer:** The user is not authenticated.

9. **What does `403 Forbidden` mean?**

   **Answer:** The user is authenticated but not allowed to perform the action.

10. **What does `404 Not Found` mean?**

    **Answer:** The requested resource does not exist.

## 6. SQL Questions

1. **What is a relational database?**

   **Answer:** It stores data in related tables with rows and columns.

2. **What is a primary key?**

   **Answer:** A unique identifier for each row.

3. **What is a foreign key?**

   **Answer:** A column that links one table to another table.

4. **Write a SELECT by ID query.**

   **Answer:**

   ```sql
   SELECT id, name, email
   FROM users
   WHERE id = 1;
   ```

5. **Write an INSERT user query.**

   **Answer:**

   ```sql
   INSERT INTO users (name, email)
   VALUES ('Alex', 'user@example.test');
   ```

6. **Write an UPDATE user query.**

   **Answer:**

   ```sql
   UPDATE users
   SET name = 'Updated User'
   WHERE id = 1;
   ```

7. **Write a DELETE user query.**

   **Answer:**

   ```sql
   DELETE FROM users
   WHERE id = 1;
   ```

8. **What is an INNER JOIN?**

   **Answer:** It returns rows that have matching records in both tables.

   ```sql
   SELECT users.name, orders.total_amount
   FROM users
   INNER JOIN orders ON orders.user_id = users.id;
   ```

9. **What is a LEFT JOIN?**

   **Answer:** It returns all rows from the left table and matching rows from the right table.

10. **What is an index?**

    **Answer:** An index helps the database find rows faster.

    ```sql
    CREATE INDEX idx_users_email
    ON users(email);
    ```

## 7. Debugging Scenarios

1. **Scenario: The frontend gets `500 Internal Server Error` from `POST /api/users`. What do you check?**

   **Answer:** Check the request body, backend logs, validation logic, database call, environment variables, and recent code changes.

   **Explanation:** A `500` usually means the backend failed unexpectedly.

2. **Scenario: The browser blocks a request with a CORS error. What do you check?**

   **Answer:** Check the backend CORS configuration, frontend API URL, allowed origins, allowed headers, and HTTP method.

   **Explanation:** CORS is enforced by the browser when frontend and backend origins differ.

3. **Scenario: A React list renders but shows stale data after adding a user. What do you check?**

   **Answer:** Check whether state is updated immutably, whether the POST response is used, and whether the component re-fetches or appends the created user.

   ```jsx
   setUsers((currentUsers) => [...currentUsers, createdUser]);
   ```

4. **Scenario: A SQL UPDATE changed every user. What probably happened?**

   **Answer:** The query probably missed a `WHERE` clause.

   ```sql
   UPDATE users
   SET active = false
   WHERE id = 1;
   ```

5. **Scenario: `req.body` is `undefined` in Express. What do you check?**

   **Answer:** Check that `app.use(express.json())` is registered before the routes and that the client sends `Content-Type: application/json`.

## 8. Short Coding Tasks

1. **Build `GET /health` endpoint.**

   **Expected solution direction:** Create a simple route that returns `200 OK` and JSON status.

   ```javascript
   app.get("/health", (req, res) => {
     res.status(200).json({ status: "ok" });
   });
   ```

2. **Build `GET /api/users` endpoint.**

   **Expected solution direction:** Return an array of users from memory, a service, or a database.

   ```javascript
   const users = [
     { id: 1, name: "Alex", email: "user@example.test" },
     { id: 2, name: "Example User", email: "secondary.user@example.test" },
   ];

   app.get("/api/users", (req, res) => {
     res.json(users);
   });
   ```

3. **Build `POST /api/users` with validation.**

   **Expected solution direction:** Read `req.body`, validate required fields, return `400` for invalid input, and return `201` for created user.

   ```javascript
   app.post("/api/users", (req, res) => {
     const { name, email } = req.body;

     if (!name || !email) {
       return res.status(400).json({ message: "Name and email are required" });
     }

     const user = {
       id: users.length + 1,
       name,
       email,
     };

     users.push(user);
     res.status(201).json(user);
   });
   ```

4. **Build `PATCH /api/users/:id`.**

   **Expected solution direction:** Read `id` from `req.params`, find the user, return `404` if missing, update selected fields, and return the updated user.

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

5. **Build a React component that fetches and renders users.**

   **Expected solution direction:** Use `useEffect` for the API call, store data in state, handle loading and error states, and render users with `map()`.

   ```jsx
   import { useEffect, useState } from "react";

   export function UsersPage() {
     const [users, setUsers] = useState([]);
     const [isLoading, setIsLoading] = useState(true);
     const [error, setError] = useState("");

     useEffect(() => {
       async function loadUsers() {
         try {
           const response = await fetch("/api/users");

           if (!response.ok) {
             throw new Error("Failed to load users");
           }

           const data = await response.json();
           setUsers(data);
         } catch (err) {
           setError(err.message);
         } finally {
           setIsLoading(false);
         }
       }

       loadUsers();
     }, []);

     if (isLoading) {
       return <p>Loading...</p>;
     }

     if (error) {
       return <p>{error}</p>;
     }

     return (
       <ul>
         {users.map((user) => (
           <li key={user.id}>{user.name}</li>
         ))}
       </ul>
     );
   }
   ```
