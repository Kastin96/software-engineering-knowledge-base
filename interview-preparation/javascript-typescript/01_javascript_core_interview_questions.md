# JavaScript Core Interview Questions

## What this document covers

This document covers beginner-to-mid-level JavaScript core concepts that are commonly asked in Software Engineer and Full-Stack interviews. The answers are short, practical, and focused on what you should be able to explain clearly during an assessment.

## Interview Questions

1. **What is JavaScript?**

   JavaScript is a programming language mainly used to make web pages interactive. It also runs on servers with Node.js, so it is widely used in full-stack development.

2. **What is the difference between `let`, `const`, and `var`?**

   `let` is used for variables that can change. `const` is used for variables that should not be reassigned. `var` is older and function-scoped, so it is usually avoided in modern JavaScript.

   ```javascript
   let count = 1;
   count = 2;

   const name = "Alex";
   // name = "Example User"; // Error

   var oldStyle = true;
   ```

3. **What are primitive types and reference types?**

   Primitive types store simple values, such as strings, numbers, booleans, `null`, `undefined`, `bigint`, and `symbol`. Reference types store references to objects, arrays, and functions.

   ```javascript
   const age = 30; // primitive
   const user = { name: "Alex" }; // reference type
   ```

4. **What are truthy and falsy values?**

   A truthy value behaves like `true` in a condition. A falsy value behaves like `false`. Common falsy values are `false`, `0`, `""`, `null`, `undefined`, and `NaN`.

   ```javascript
   if ("hello") {
     console.log("This is truthy");
   }

   if (!0) {
     console.log("0 is falsy");
   }
   ```

5. **What is the difference between `==` and `===`?**

   `==` compares values after type conversion. `===` compares both value and type. In interviews and real projects, prefer `===` because it is safer and clearer.

   ```javascript
   console.log(5 == "5"); // true
   console.log(5 === "5"); // false
   ```

6. **What is hoisting?**

   Hoisting means JavaScript moves declarations to the top of their scope before running the code. `var` is hoisted with `undefined`, while `let` and `const` are hoisted but cannot be used before declaration.

   ```javascript
   console.log(value); // undefined
   var value = 10;
   ```

7. **What is scope?**

   Scope defines where a variable can be accessed. JavaScript has global scope, function scope, and block scope.

   ```javascript
   const globalValue = "available everywhere in this file";

   function testScope() {
     const functionValue = "available inside this function";
   }

   if (true) {
     let blockValue = "available inside this block";
   }
   ```

8. **What is a closure?**

   A closure happens when a function remembers variables from the scope where it was created, even after that outer function has finished.

   ```javascript
   function createCounter() {
     let count = 0;

     return function increment() {
       count += 1;
       return count;
     };
   }

   const counter = createCounter();

   console.log(counter()); // 1
   console.log(counter()); // 2
   ```

9. **What is the spread operator?**

   The spread operator `...` expands arrays or objects. It is often used to copy, combine, or pass values.

   ```javascript
   const numbers = [1, 2, 3];
   const moreNumbers = [...numbers, 4, 5];

   console.log(moreNumbers); // [1, 2, 3, 4, 5]

   const user = { name: "Alex", role: "Developer" };
   const updatedUser = { ...user, role: "Senior Developer" };

   console.log(updatedUser);
   ```

10. **What is destructuring?**

    Destructuring lets you extract values from arrays or objects into variables.

    ```javascript
    const user = {
      name: "Alex",
      role: "Developer",
    };

    const { name, role } = user;

    console.log(name); // Alex
    console.log(role); // Developer

    const numbers = [10, 20];
    const [first, second] = numbers;

    console.log(first); // 10
    console.log(second); // 20
    ```

11. **What are template literals?**

    Template literals use backticks and allow variables or expressions inside strings with `${}`. They are useful for readable string formatting.

    ```javascript
    const name = "Alex";
    const message = `Hello, ${name}!`;

    console.log(message); // Hello, Alex!
    ```

12. **What is optional chaining?**

    Optional chaining `?.` lets you safely access nested object properties without throwing an error if something is `null` or `undefined`.

    ```javascript
    const user = {};

    console.log(user.profile?.email); // undefined
    ```

13. **What is nullish coalescing?**

    Nullish coalescing `??` returns the right-side value only when the left-side value is `null` or `undefined`.

    ```javascript
    const username = null;
    const displayName = username ?? "Guest";

    console.log(displayName); // Guest
    ```

14. **What is the difference between `null` and `undefined`?**

    `undefined` usually means a value has not been assigned. `null` is an intentional empty value set by the developer.

    ```javascript
    let value;
    console.log(value); // undefined

    const selectedUser = null;
    console.log(selectedUser); // null
    ```

15. **What are callbacks?**

    A callback is a function passed into another function to be executed later.

    ```javascript
    function greet(name, callback) {
      callback(`Hello, ${name}`);
    }

    greet("Alex", function (message) {
      console.log(message);
    });
    ```

16. **What are higher-order functions?**

    A higher-order function is a function that takes another function as an argument or returns a function. Many array methods, such as `map` and `filter`, are higher-order functions.

    ```javascript
    const numbers = [1, 2, 3];
    const doubled = numbers.map(function (number) {
      return number * 2;
    });

    console.log(doubled); // [2, 4, 6]
    ```

17. **What are common array methods: `map`, `filter`, `reduce`, `find`, `some`, and `every`?**

    `map` creates a new array by transforming each item. `filter` creates a new array with matching items. `reduce` combines values into one result. `find` returns the first matching item. `some` checks if at least one item matches. `every` checks if all items match.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    const doubled = numbers.map((number) => number * 2);
    console.log(doubled); // [2, 4, 6, 8, 10]

    const evenNumbers = numbers.filter((number) => number % 2 === 0);
    console.log(evenNumbers); // [2, 4]

    const total = numbers.reduce((sum, number) => sum + number, 0);
    console.log(total); // 15

    const firstLargeNumber = numbers.find((number) => number > 3);
    console.log(firstLargeNumber); // 4

    const hasEvenNumber = numbers.some((number) => number % 2 === 0);
    console.log(hasEvenNumber); // true

    const allPositive = numbers.every((number) => number > 0);
    console.log(allPositive); // true
    ```

18. **What is the difference between `forEach` and `map`?**

    `forEach` runs a function for each item but does not return a new array. `map` returns a new array with transformed values.

    ```javascript
    const numbers = [1, 2, 3];

    numbers.forEach((number) => console.log(number));

    const doubled = numbers.map((number) => number * 2);
    console.log(doubled); // [2, 4, 6]
    ```

19. **How do you clone an object?**

    For a shallow clone, use the spread operator or `Object.assign`. A shallow clone copies only the first level.

    ```javascript
    const user = { name: "Alex", role: "Developer" };

    const copy = { ...user };
    console.log(copy);
    ```

20. **How do you merge objects?**

    You can merge objects using the spread operator. If the same key exists in multiple objects, the later value wins.

    ```javascript
    const baseUser = { name: "Alex", active: true };
    const userDetails = { role: "Developer", active: false };

    const mergedUser = { ...baseUser, ...userDetails };

    console.log(mergedUser);
    // { name: "Alex", active: false, role: "Developer" }
    ```

21. **What is immutability?**

    Immutability means not changing the original value directly. Instead, you create a new value with the changes. This is common in React and state management.

    ```javascript
    const user = { name: "Alex", role: "Developer" };

    const updatedUser = { ...user, role: "Team Lead" };

    console.log(user.role); // Developer
    console.log(updatedUser.role); // Team Lead
    ```

22. **What is the event loop in simple words?**

    The event loop is how JavaScript handles asynchronous work. JavaScript runs synchronous code first, then handles queued tasks such as timers, promises, and events when the main call stack is free.

    ```javascript
    console.log("Start");

    setTimeout(() => {
      console.log("Timer");
    }, 0);

    console.log("End");

    // Output:
    // Start
    // End
    // Timer
    ```

23. **What is the difference between synchronous and asynchronous code?**

    Synchronous code runs line by line and blocks the next line until it finishes. Asynchronous code can start a task and continue running other code while waiting for the result.

    ```javascript
    console.log("Sync 1");
    console.log("Sync 2");

    console.log("Async start");

    setTimeout(() => {
      console.log("Async result");
    }, 1000);

    console.log("Async end");

    // Output:
    // Sync 1
    // Sync 2
    // Async start
    // Async end
    // Async result
    ```

24. **What are common JavaScript mistakes in interviews?**

    Common mistakes include confusing `==` and `===`, using `var` without understanding scope, mutating objects accidentally, forgetting that array methods return new arrays, misunderstanding asynchronous output order, and not knowing the difference between `null` and `undefined`.

    ```javascript
    const numbers = [1, 2, 3];

    // Mistake: expecting forEach to return a new array
    const result = numbers.forEach((number) => number * 2);

    console.log(result); // undefined
    ```
