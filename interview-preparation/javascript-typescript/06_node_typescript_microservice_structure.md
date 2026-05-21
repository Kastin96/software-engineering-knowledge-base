# Node.js and TypeScript Microservice Structure

## What this document covers

This document explains how to structure a small Node.js and TypeScript REST microservice. It is written for someone with a Java/Spring Boot background, so the examples compare common Node.js folders to familiar layers such as controllers, services, repositories, DTOs, and middleware.

## Example Project Structure

```text
src/
  app.ts
  server.ts
  models/
  routes/
  controllers/
  services/
  repositories/
  validators/
  middleware/
```

In a small service, this structure keeps HTTP logic, business logic, data access, validation, and error handling separated.

## Interview Questions

1. **What is a microservice?**

   A microservice is a small application that owns one business capability. It usually has its own API, data model, and deployment process.

2. **What is a REST microservice?**

   A REST microservice exposes resources through HTTP endpoints such as `GET`, `POST`, `PATCH`, and `DELETE`. It usually sends and receives JSON.

3. **What is a typical Node.js and TypeScript project structure?**

   A typical structure separates the app into routes, controllers, services, repositories, validators, middleware, and models or types.

   ```text
   src/
     app.ts
     server.ts
     models/
     routes/
     controllers/
     services/
     repositories/
     validators/
     middleware/
   ```

4. **What is the `routes` folder for?**

   The `routes` folder maps HTTP methods and URLs to controller functions. It is similar to route annotations in Spring controllers, but usually separated into router files.

   ```typescript
   import { Router } from "express";
   import { createUser } from "../controllers/userController";

   export const userRouter = Router();

   userRouter.post("/users", createUser);
   ```

5. **What is the `controllers` folder for?**

   Controllers handle HTTP request and response logic. They read data from `req`, call services, and return responses.

6. **What is the `services` folder for?**

   Services contain business logic. They decide what the application should do, but they should not know too much about HTTP details.

7. **What is the `repositories` folder for?**

   Repositories handle data access. In a real service, they may call a database. In a simple interview example, they can use an in-memory array.

8. **What is the `validators` folder for?**

   Validators check incoming request data before it reaches business logic. This is similar to using validation annotations or request validators in Spring Boot.

9. **What is the `middleware` folder for?**

   Middleware contains functions that run during the request-response cycle. Common examples include logging, authentication, request validation, and error handling.

10. **What is the `models` or `types` folder for?**

    This folder contains TypeScript interfaces and types, such as domain models, DTOs, and request or response shapes.

11. **What is the difference between controller and service?**

    A controller handles HTTP concerns. A service handles business logic. For example, the controller reads `req.body`, while the service decides how to create a user.

12. **What is the difference between service and repository?**

    A service contains business rules. A repository reads and writes data. The service calls the repository when it needs data access.

13. **Why keep business logic out of controllers?**

    Keeping business logic out of controllers makes the code easier to test, reuse, and maintain. Controllers stay small and focused on HTTP behavior.

14. **Why centralize error handling?**

    Centralized error handling gives the API consistent error responses and avoids repeating `try/catch` logic in every route.

15. **Why use DTO/request types?**

    DTOs and request types document what data the API expects. They also help TypeScript catch mistakes when reading or passing request data.

16. **How is this structure similar to Spring Boot?**

    The layers are very similar. Express routes and controllers are like Spring controllers. Services are like `@Service` classes. Repositories are like `@Repository` classes. DTO or request interfaces are like request DTO classes.

17. **How can you explain this structure in an interview?**

    A clear answer is: "Routes define endpoints, controllers handle HTTP, services contain business logic, repositories handle data access, validators check input, models define types, and middleware handles cross-cutting concerns like errors or authentication."

18. **What are common design mistakes in small Node.js services?**

    Common mistakes include putting all code in `app.ts`, writing business logic directly in route handlers, skipping validation, returning inconsistent errors, mixing database code into controllers, and not defining request or response types.

## Small User Example

This example shows a simple user flow using TypeScript interfaces, a controller, a service, a repository, and error middleware.

### `models/user.ts`

```typescript
export interface User {
  id: number;
  email: string;
  name: string;
  active: boolean;
}

export interface CreateUserRequest {
  email: string;
  name: string;
}
```

### `repositories/userRepository.ts`

```typescript
import { User } from "../models/user";

const users: User[] = [];

export function findAllUsers(): User[] {
  return users;
}

export function createUserRecord(user: Omit<User, "id">): User {
  const newUser: User = {
    id: users.length + 1,
    ...user,
  };

  users.push(newUser);
  return newUser;
}
```

### `services/userService.ts`

```typescript
import { CreateUserRequest, User } from "../models/user";
import { createUserRecord, findAllUsers } from "../repositories/userRepository";

export function getUsers(): User[] {
  return findAllUsers();
}

export function createUser(request: CreateUserRequest): User {
  if (!request.email || !request.name) {
    throw new Error("Email and name are required");
  }

  return createUserRecord({
    email: request.email,
    name: request.name,
    active: true,
  });
}
```

### `controllers/userController.ts`

```typescript
import { Request, Response, NextFunction } from "express";
import { CreateUserRequest } from "../models/user";
import { createUser as createUserService, getUsers } from "../services/userService";

export function listUsers(req: Request, res: Response): void {
  const users = getUsers();
  res.json(users);
}

export function createUser(
  req: Request<{}, {}, CreateUserRequest>,
  res: Response,
  next: NextFunction
): void {
  try {
    const user = createUserService(req.body);
    res.status(201).json(user);
  } catch (error) {
    next(error);
  }
}
```

### `routes/userRoutes.ts`

```typescript
import { Router } from "express";
import { createUser, listUsers } from "../controllers/userController";

export const userRouter = Router();

userRouter.get("/users", listUsers);
userRouter.post("/users", createUser);
```

### `middleware/errorMiddleware.ts`

```typescript
import { Request, Response, NextFunction } from "express";

export function errorMiddleware(
  err: Error,
  req: Request,
  res: Response,
  next: NextFunction
): void {
  res.status(500).json({
    message: err.message || "Internal server error",
  });
}
```

### `app.ts`

```typescript
import express from "express";
import { userRouter } from "./routes/userRoutes";
import { errorMiddleware } from "./middleware/errorMiddleware";

export const app = express();

app.use(express.json());
app.use("/api", userRouter);
app.use(errorMiddleware);
```

### `server.ts`

```typescript
import { app } from "./app";

const port = process.env.PORT || 3000;

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```
