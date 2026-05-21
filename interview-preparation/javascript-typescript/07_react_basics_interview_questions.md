# React Basics Interview Questions

## What this document covers

This document covers React basics for full-stack interviews and assessments. It focuses on components, props, state, hooks, rendering, forms, and common beginner-to-mid-level mistakes.

## Interview Questions

1. **What is React?**

   React is a JavaScript library for building user interfaces. It is commonly used to create reusable components and update the UI when data changes.

2. **What is a component?**

   A component is a reusable piece of UI. It can receive data, manage state, and return JSX.

   ```jsx
   function WelcomeMessage() {
     return <h1>Welcome</h1>;
   }
   ```

3. **What are functional components?**

   Functional components are JavaScript functions that return React UI. Modern React mainly uses functional components with hooks.

   ```jsx
   function UserCard() {
     return <div>User profile</div>;
   }
   ```

4. **What are props?**

   Props are values passed from a parent component to a child component. They are read-only inside the child.

   ```jsx
   function UserCard({ name }) {
     return <p>User: {name}</p>;
   }

   function App() {
     return <UserCard name="Alex" />;
   }
   ```

5. **What is state?**

   State is data that belongs to a component and can change over time. When state changes, React re-renders the component.

6. **What is the difference between props and state?**

   Props are passed into a component from its parent. State is managed inside the component. Props are read-only, while state can be updated with a setter function.

7. **What is JSX?**

   JSX is a syntax that lets you write HTML-like code inside JavaScript. React uses JSX to describe what the UI should look like.

   ```jsx
   const element = <h1>Hello React</h1>;
   ```

8. **What is `useState`?**

   `useState` is a React hook used to store and update component state.

   ```jsx
   import { useState } from "react";

   function Counter() {
     const [count, setCount] = useState(0);

     return (
       <button onClick={() => setCount(count + 1)}>
         Count: {count}
       </button>
     );
   }
   ```

9. **What is `useEffect`?**

   `useEffect` is a React hook used to run side effects, such as fetching data, setting up subscriptions, or updating the document title.

   ```jsx
   import { useEffect, useState } from "react";

   function Users() {
     const [users, setUsers] = useState([]);

     useEffect(() => {
       fetch("/api/users")
         .then((response) => response.json())
         .then((data) => setUsers(data));
     }, []);

     return <p>Total users: {users.length}</p>;
   }
   ```

10. **What is conditional rendering?**

    Conditional rendering means showing different UI based on a condition.

    ```jsx
    function LoginStatus({ isLoggedIn }) {
      if (isLoggedIn) {
        return <p>Welcome back</p>;
      }

      return <p>Please log in</p>;
    }
    ```

11. **How do you render a list?**

    Use `map()` to transform an array into JSX elements.

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

12. **Why do lists need the `key` prop?**

    Keys help React identify which list items changed, were added, or were removed. Use a stable unique value, such as an ID.

    ```jsx
    users.map((user) => <li key={user.id}>{user.name}</li>);
    ```

13. **What are controlled components?**

    A controlled component is a form input whose value is controlled by React state.

    ```jsx
    import { useState } from "react";

    function NameForm() {
      const [name, setName] = useState("");

      return (
        <input
          value={name}
          onChange={(event) => setName(event.target.value)}
        />
      );
    }
    ```

14. **How do you handle form input?**

    Store the input value in state and update it in the `onChange` handler.

    ```jsx
    function EmailInput() {
      const [email, setEmail] = useState("");

      return (
        <input
          type="email"
          value={email}
          onChange={(event) => setEmail(event.target.value)}
        />
      );
    }
    ```

15. **How do you handle a button click?**

    Pass a function to the `onClick` prop.

    ```jsx
    function SaveButton() {
      function handleClick() {
        console.log("Saved");
      }

      return <button onClick={handleClick}>Save</button>;
    }
    ```

16. **What is parent-to-child data flow?**

    Parent-to-child data flow means the parent passes data to the child using props.

    ```jsx
    function Child({ message }) {
      return <p>{message}</p>;
    }

    function Parent() {
      return <Child message="Hello from parent" />;
    }
    ```

17. **What is a child-to-parent callback?**

    A child-to-parent callback means the parent passes a function to the child, and the child calls it to send data or trigger an action.

    ```jsx
    function Child({ onSelect }) {
      return <button onClick={() => onSelect("Alex")}>Select Alex</button>;
    }

    function Parent() {
      function handleSelect(name) {
        console.log("Selected:", name);
      }

      return <Child onSelect={handleSelect} />;
    }
    ```

18. **What is a component re-render?**

    A re-render means React runs the component again to calculate the updated UI. React then updates the browser only where the UI actually changed.

19. **What causes a re-render?**

    Common causes include state changes, new props from a parent, context changes, and parent component re-renders.

20. **What are common React mistakes in interviews?**

    Common mistakes include mutating state directly, forgetting the `key` prop in lists, using array indexes as keys when items can change, calling state setters during render, misunderstanding `useEffect` dependencies, and mixing controlled and uncontrolled inputs.

    ```jsx
    // Mistake: mutating state directly
    users.push(newUser);
    setUsers(users);

    // Better: create a new array
    setUsers([...users, newUser]);
    ```

## Compact Example

This example combines props, state, effects, controlled input, list rendering, and conditional rendering.

```jsx
import { useEffect, useState } from "react";

function UserList({ users }) {
  if (users.length === 0) {
    return <p>No users found</p>;
  }

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

export function App() {
  const [users, setUsers] = useState([]);
  const [name, setName] = useState("");

  useEffect(() => {
    setUsers([{ id: 1, name: "Alex" }]);
  }, []);

  function handleAddUser() {
    const user = {
      id: users.length + 1,
      name,
    };

    setUsers([...users, user]);
    setName("");
  }

  return (
    <div>
      <input
        value={name}
        onChange={(event) => setName(event.target.value)}
        placeholder="User name"
      />

      <button onClick={handleAddUser}>Add user</button>

      <UserList users={users} />
    </div>
  );
}
```
