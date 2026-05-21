# React API Integration Interview Questions

## What this document covers

This document covers how React applications communicate with backend APIs. It focuses on `fetch`, `useEffect`, loading and error states, form submission, JSON requests, CORS, and separating API logic from UI code.

## Interview Questions

1. **How does React call a backend API?**

   React usually calls a backend API with `fetch` or a library like Axios. The API call returns data, and React stores that data in state so it can render the UI.

2. **What is `fetch`?**

   `fetch` is a browser API used to make HTTP requests. It returns a Promise that resolves to a response object.

   ```jsx
   fetch("/api/users")
     .then((response) => response.json())
     .then((users) => console.log(users));
   ```

3. **How do you call an API inside `useEffect`?**

   Put the API call inside `useEffect` so it runs after the component renders. Use an empty dependency array when the call should run once on mount.

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

     return <p>Total users: {users.length}</p>;
   }
   ```

4. **How do you handle loading state?**

   Use a state variable such as `isLoading` to show a loading message or spinner while the request is running.

   ```jsx
   const [isLoading, setIsLoading] = useState(false);

   async function loadUsers() {
     setIsLoading(true);
     const response = await fetch("/api/users");
     const users = await response.json();
     setUsers(users);
     setIsLoading(false);
   }
   ```

5. **How do you handle error state?**

   Store the error message in state and render it when a request fails.

   ```jsx
   const [error, setError] = useState("");

   async function loadUsers() {
     try {
       const response = await fetch("/api/users");

       if (!response.ok) {
         throw new Error("Failed to load users");
       }

       const users = await response.json();
       setUsers(users);
     } catch (err) {
       setError(err.message);
     }
   }
   ```

6. **How do you render API data?**

   Store the API response in state, then render it with JSX. For arrays, use `map()`.

   ```jsx
   function UserList({ users }) {
     return (
       <ul>
         {users.map((user) => (
           <li key={user.id}>{user.name}</li>
         ))}
       </ul>
     );
   }
   ```

7. **How do you submit a form to the backend?**

   Handle the form `onSubmit`, prevent the default browser reload, then send the form data with a POST request.

   ```jsx
   async function handleSubmit(event) {
     event.preventDefault();

     await fetch("/api/users", {
       method: "POST",
       headers: {
         "Content-Type": "application/json",
       },
       body: JSON.stringify({ name, email }),
     });
   }
   ```

8. **How do you send a JSON body?**

   Use `JSON.stringify()` to convert a JavaScript object into a JSON string.

   ```jsx
   const body = JSON.stringify({
     name: "Alex",
     email: "user@example.test",
   });
   ```

9. **How do you set the `Content-Type` header?**

   Set `"Content-Type": "application/json"` in the request headers when sending JSON.

   ```jsx
   fetch("/api/users", {
     method: "POST",
     headers: {
       "Content-Type": "application/json",
     },
     body: JSON.stringify({ name: "Alex" }),
   });
   ```

10. **How do you update the UI after `POST`?**

    Use the response from the backend to update React state. For example, add the created user to the existing users array.

    ```jsx
    const response = await fetch("/api/users", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ name, email }),
    });

    const createdUser = await response.json();
    setUsers((currentUsers) => [...currentUsers, createdUser]);
    ```

11. **How do you handle a failed request?**

    Check `response.ok`. If it is false, throw an error and handle it with `catch`.

    ```jsx
    async function requestUsers() {
      try {
        const response = await fetch("/api/users");

        if (!response.ok) {
          throw new Error(`Request failed: ${response.status}`);
        }

        return await response.json();
      } catch (error) {
        console.log(error.message);
        return [];
      }
    }
    ```

12. **What is the difference between frontend validation and backend validation?**

    Frontend validation improves user experience by catching simple mistakes early. Backend validation protects the system and must always be used because frontend code can be bypassed.

13. **What is CORS in simple words?**

    CORS is a browser security rule that controls whether a frontend from one origin can call an API from another origin. If CORS is not configured on the backend, the browser may block the request.

14. **Why should API calls not be directly mixed with complex UI logic?**

    Separating API calls from complex UI logic makes components easier to read, test, and reuse. It also keeps request behavior consistent across the app.

15. **How do you separate API client functions?**

    Create helper functions that handle requests, errors, and JSON parsing. Components can call these helpers instead of repeating `fetch` logic.

    ```jsx
    export async function getUsers() {
      const response = await fetch("/api/users");

      if (!response.ok) {
        throw new Error("Failed to load users");
      }

      return response.json();
    }

    export async function createUser(user) {
      const response = await fetch("/api/users", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(user),
      });

      if (!response.ok) {
        throw new Error("Failed to create user");
      }

      return response.json();
    }
    ```

16. **What are common mistakes in React API calls?**

    Common mistakes include forgetting the dependency array in `useEffect`, not handling loading or error states, not checking `response.ok`, forgetting `JSON.stringify`, missing the `Content-Type` header, directly mutating state after a response, and putting too much API logic inside JSX.

## Compact API Integration Example

This example shows GET users, loading and error state, rendering returned data, and POST form submission.

```jsx
import { useEffect, useState } from "react";

async function getUsers() {
  const response = await fetch("/api/users");

  if (!response.ok) {
    throw new Error("Failed to load users");
  }

  return response.json();
}

async function createUser(user) {
  const response = await fetch("/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(user),
  });

  if (!response.ok) {
    throw new Error("Failed to create user");
  }

  return response.json();
}

export function UsersPage() {
  const [users, setUsers] = useState([]);
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState("");

  useEffect(() => {
    async function loadUsers() {
      try {
        const data = await getUsers();
        setUsers(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    }

    loadUsers();
  }, []);

  async function handleSubmit(event) {
    event.preventDefault();
    setError("");

    try {
      const createdUser = await createUser({ name, email });
      setUsers((currentUsers) => [...currentUsers, createdUser]);
      setName("");
      setEmail("");
    } catch (err) {
      setError(err.message);
    }
  }

  if (isLoading) {
    return <p>Loading users...</p>;
  }

  return (
    <section>
      {error && <p>{error}</p>}

      <form onSubmit={handleSubmit}>
        <input
          value={name}
          onChange={(event) => setName(event.target.value)}
          placeholder="Name"
        />

        <input
          value={email}
          onChange={(event) => setEmail(event.target.value)}
          placeholder="Email"
        />

        <button type="submit">Create user</button>
      </form>

      <ul>
        {users.map((user) => (
          <li key={user.id}>
            {user.name} - {user.email}
          </li>
        ))}
      </ul>
    </section>
  );
}
```
