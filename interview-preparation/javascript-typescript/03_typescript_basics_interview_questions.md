# TypeScript Basics Interview Questions

## What this document covers

This document covers TypeScript basics for full-stack interviews. It is written for developers who already understand backend programming but are newer to TypeScript and typed JavaScript projects.

## Interview Questions

1. **What is TypeScript?**

   TypeScript is a typed version of JavaScript. It adds types, interfaces, and compile-time checks, then compiles to regular JavaScript.

   ```typescript
   const message: string = "Hello TypeScript";
   console.log(message);
   ```

2. **Why use TypeScript instead of JavaScript?**

   TypeScript helps catch errors before runtime, improves autocomplete, makes refactoring safer, and documents the shape of data in the code.

3. **What is static typing?**

   Static typing means variable and function types are checked before the code runs. This helps find mistakes during development instead of in production.

   ```typescript
   let age: number = 30;

   // age = "thirty"; // Error
   ```

4. **What are type annotations?**

   Type annotations explicitly tell TypeScript what type a value should have.

   ```typescript
   const username: string = "alex";
   const isActive: boolean = true;
   const loginCount: number = 5;
   ```

5. **What is type inference?**

   Type inference means TypeScript can guess the type from the assigned value. You do not always need to write the type manually.

   ```typescript
   const username = "alex"; // inferred as string
   const loginCount = 5; // inferred as number
   ```

6. **What is the difference between `type` and `interface`?**

   Both can describe object shapes. `interface` is often used for object contracts and can be extended. `type` is more flexible and can also define unions, literals, and other type combinations.

   ```typescript
   interface UserInterface {
     id: number;
     email: string;
   }

   type UserType = {
     id: number;
     email: string;
   };

   type Status = "active" | "inactive";
   ```

7. **What are optional properties?**

   Optional properties use `?` and may be missing from an object.

   ```typescript
   interface User {
     id: number;
     email: string;
     displayName?: string;
   }

   const user: User = {
     id: 1,
     email: "user@example.test",
   };
   ```

8. **What are union types?**

   A union type allows a value to be one of several types.

   ```typescript
   let userId: string | number;

   userId = "abc-123";
   userId = 123;
   ```

9. **What are literal types?**

   Literal types allow only specific values. They are useful for statuses, roles, and fixed options.

   ```typescript
   type UserRole = "admin" | "manager" | "user";

   const role: UserRole = "admin";
   ```

10. **How do arrays work in TypeScript?**

    Arrays can be typed so every item must match the expected type.

    ```typescript
    const numbers: number[] = [1, 2, 3];
    const names: Array<string> = ["Alex", "Example User"];
    ```

11. **How do you type function parameters?**

    Add type annotations to function parameters so callers must pass the correct values.

    ```typescript
    function greetUser(name: string, loginCount: number) {
      return `Hello ${name}, logins: ${loginCount}`;
    }
    ```

12. **How do you type function return values?**

    Add a type after the parameter list to define what the function returns.

    ```typescript
    function add(a: number, b: number): number {
      return a + b;
    }
    ```

13. **What are `void`, `unknown`, `any`, and `never`?**

    `void` means a function does not return a useful value. `unknown` means the value type is not known yet and must be checked before use. `any` disables type checking and should be avoided when possible. `never` means a value should never occur, often used for functions that throw errors.

    ```typescript
    function logMessage(message: string): void {
      console.log(message);
    }

    function parseInput(input: unknown): string {
      if (typeof input === "string") {
        return input;
      }

      return "Invalid input";
    }

    function fail(message: string): never {
      throw new Error(message);
    }
    ```

14. **What are object types?**

    Object types describe the expected properties and property types of an object.

    ```typescript
    const user: { id: number; email: string } = {
      id: 1,
      email: "user@example.test",
    };
    ```

15. **What are type aliases?**

    A type alias gives a name to a type. This makes code easier to read and reuse.

    ```typescript
    type UserId = string | number;

    type User = {
      id: UserId;
      email: string;
      isActive: boolean;
    };
    ```

16. **How can interfaces be used for API DTOs?**

    Interfaces are useful for defining request and response data shapes. This helps keep API code clear and safer.

    ```typescript
    interface User {
      id: number;
      email: string;
      name: string;
      isActive: boolean;
    }

    interface CreateUserRequest {
      email: string;
      name: string;
      password: string;
    }

    interface UpdateUserRequest {
      email?: string;
      name?: string;
      isActive?: boolean;
    }
    ```

17. **What are generics in TypeScript?**

    Generics let you write reusable types or functions while keeping type safety.

    ```typescript
    function wrapInArray<T>(value: T): T[] {
      return [value];
    }

    const numbers = wrapInArray<number>(5);
    const names = wrapInArray<string>("Alex");
    ```

18. **What is `Promise<T>`?**

    `Promise<T>` describes the type of value that an async function will eventually return.

    ```typescript
    interface User {
      id: number;
      email: string;
    }

    async function getUsers(): Promise<User[]> {
      return [
        { id: 1, email: "user@example.test" },
        { id: 2, email: "secondary.user@example.test" },
      ];
    }
    ```

19. **What is `Partial<T>`?**

    `Partial<T>` makes all properties of a type optional. It is useful for update requests where only some fields may be sent.

    ```typescript
    interface User {
      id: number;
      email: string;
      name: string;
      isActive: boolean;
    }

    const updateUser = (id: number, changes: Partial<User>): User => {
      const existingUser: User = {
        id,
        email: "user@example.test",
        name: "Alex",
        isActive: true,
      };

      return { ...existingUser, ...changes };
    };

    const updated = updateUser(1, { name: "Updated User" });
    ```

20. **What are `Pick<T>` and `Omit<T>`?**

    `Pick<T>` creates a type with only selected properties. `Omit<T>` creates a type without selected properties.

    ```typescript
    interface User {
      id: number;
      email: string;
      name: string;
      passwordHash: string;
    }

    type PublicUser = Omit<User, "passwordHash">;
    type LoginRequest = Pick<User, "email"> & {
      password: string;
    };
    ```

21. **How can TypeScript be used with an Express request body?**

    You can type the request body so your route handler knows what fields to expect.

    ```typescript
    import { Request, Response } from "express";

    interface CreateUserRequest {
      email: string;
      name: string;
      password: string;
    }

    function createUserHandler(
      req: Request<{}, {}, CreateUserRequest>,
      res: Response
    ): void {
      const { email, name, password } = req.body;

      res.status(201).json({
        email,
        name,
        passwordLength: password.length,
      });
    }
    ```

22. **What are common TypeScript mistakes?**

    Common mistakes include using `any` too often, adding types everywhere when inference is enough, ignoring `undefined`, confusing `type` and `interface`, not typing API request and response objects, and using type assertions to hide real errors.

    ```typescript
    // Risky: this hides possible runtime problems
    const value = JSON.parse("{ \"id\": 1 }") as { id: number; email: string };

    // Better: validate unknown data before trusting it
    const parsed: unknown = JSON.parse("{ \"id\": 1 }");
    ```
