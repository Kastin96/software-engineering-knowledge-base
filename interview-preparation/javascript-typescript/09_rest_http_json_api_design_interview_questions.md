# REST, HTTP, JSON, and API Design Interview Questions

## What this document covers

This document covers REST, HTTP methods, JSON, status codes, request design, error responses, pagination, filtering, sorting, versioning, and authentication basics for backend and full-stack interviews.

## Interview Questions

1. **What is REST?**

   REST is an API design style that uses HTTP methods and URLs to work with resources. A REST API usually sends and receives JSON.

2. **What is a resource?**

   A resource is a thing the API manages, such as a user, order, product, or invoice. In REST, resources are usually represented by nouns in URLs.

   ```text
   /api/users
   /api/orders
   /api/products
   ```

3. **What is an endpoint?**

   An endpoint is a specific HTTP method and URL that clients can call.

   ```text
   GET /api/users
   POST /api/users
   GET /api/users/1
   ```

4. **What are the main HTTP methods: `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`?**

   `GET` reads data. `POST` creates data. `PUT` replaces a full resource. `PATCH` updates part of a resource. `DELETE` removes a resource.

   ```text
   GET /api/users
   POST /api/users
   PUT /api/users/1
   PATCH /api/users/1
   DELETE /api/users/1
   ```

5. **What is the difference between `PUT` and `PATCH`?**

   `PUT` usually replaces the entire resource. `PATCH` updates only selected fields.

   ```json
   {
     "name": "Updated User"
   }
   ```

6. **What is JSON?**

   JSON is a text format for sending structured data. It is common in APIs because it is easy for both frontend and backend code to read.

   ```json
   {
     "id": 1,
     "name": "Alex",
     "email": "user@example.test"
   }
   ```

7. **What is the difference between request body and response body?**

   The request body is data sent from the client to the server. The response body is data sent from the server back to the client.

   ```text
   Client sends request body: create this user
   Server sends response body: created user details
   ```

8. **What is the difference between path parameters and query parameters?**

   Path parameters identify a specific resource. Query parameters usually filter, sort, search, or paginate results.

   ```text
   GET /api/users/1
   GET /api/users?page=1&size=10
   ```

9. **What are common HTTP status codes?**

   Common status codes include `200 OK`, `201 Created`, `204 No Content`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, and `500 Internal Server Error`.

10. **What is the difference between `200`, `201`, and `204`?**

    `200` means the request succeeded and usually returns data. `201` means a resource was created. `204` means the request succeeded but there is no response body.

    ```text
    GET /api/users/1 -> 200 OK
    POST /api/users -> 201 Created
    DELETE /api/users/1 -> 204 No Content
    ```

11. **What is the difference between `400`, `401`, `403`, and `404`?**

    `400` means the request is invalid. `401` means the user is not authenticated. `403` means the user is authenticated but not allowed. `404` means the resource was not found.

    ```json
    {
      "message": "User not found"
    }
    ```

12. **What is idempotency?**

    Idempotency means making the same request multiple times has the same final result as making it once.

    ```text
    DELETE /api/users/1
    DELETE /api/users/1
    ```

    After the first delete, the user is already gone. Repeating the delete should not create a different final state.

13. **Which HTTP methods are idempotent?**

    `GET`, `PUT`, and `DELETE` are usually idempotent. `PATCH` can be idempotent depending on design. `POST` is usually not idempotent because repeated calls may create multiple resources.

14. **What is request validation?**

    Request validation checks that incoming data is correct before the server processes it.

    ```json
    {
      "message": "Validation failed",
      "errors": [
        {
          "field": "email",
          "message": "Email is required"
        }
      ]
    }
    ```

15. **What is an error response format?**

    An error response format is a consistent JSON structure used when something fails.

    ```json
    {
      "message": "User not found",
      "statusCode": 404,
      "code": "USER_NOT_FOUND"
    }
    ```

16. **What is pagination?**

    Pagination splits large result sets into smaller pages. This improves performance and avoids returning too much data at once.

    ```text
    GET /api/users?page=1&size=10
    ```

    ```json
    {
      "items": [
        {
          "id": 1,
          "name": "Alex"
        }
      ],
      "page": 1,
      "size": 10,
      "totalItems": 42
    }
    ```

17. **What is filtering?**

    Filtering limits results based on criteria, such as status, role, or search text.

    ```text
    GET /api/users?active=true
    GET /api/users?role=admin
    ```

18. **What is sorting?**

    Sorting orders results by a field, such as name or creation date.

    ```text
    GET /api/users?sort=name
    GET /api/users?sort=-createdAt
    ```

19. **What is API versioning?**

    API versioning lets you change an API without breaking existing clients. A common approach is putting the version in the URL.

    ```text
    GET /api/v1/users
    GET /api/v2/users
    ```

20. **What is authentication vs authorization?**

    Authentication checks who the user is. Authorization checks what the user is allowed to do.

    ```text
    Authentication: Are you logged in?
    Authorization: Are you allowed to delete this user?
    ```

21. **What is JWT in simple words?**

    JWT stands for JSON Web Token. It is a signed token often used to prove that a user is authenticated when calling protected APIs.

    ```text
    Authorization: Bearer <token>
    ```

22. **How do you design a simple users API?**

    A simple users API should use clear resource-based endpoints, correct HTTP methods, meaningful status codes, validation, and consistent JSON responses.

    ```text
    GET /api/users
    GET /api/users/:id
    POST /api/users
    PATCH /api/users/:id
    DELETE /api/users/:id
    ```

    ```json
    {
      "id": 1,
      "name": "Alex",
      "email": "user@example.test"
    }
    ```

23. **What are common REST API mistakes?**

    Common mistakes include using verbs in URLs, returning wrong status codes, ignoring validation, returning inconsistent errors, exposing internal error details, missing pagination, using `POST` for every operation, and confusing authentication with authorization.

    ```text
    Avoid: POST /api/getUsers
    Prefer: GET /api/users
    ```

## Simple Users API Examples

### `GET /api/users`

Returns a list of users.

```json
[
  {
    "id": 1,
    "name": "Alex",
    "email": "user@example.test"
  },
  {
    "id": 2,
    "name": "Example User",
    "email": "secondary.user@example.test"
  }
]
```

### `GET /api/users/:id`

Returns one user by ID.

```json
{
  "id": 1,
  "name": "Alex",
  "email": "user@example.test"
}
```

### `POST /api/users`

Creates a new user.

```json
{
  "name": "Alex",
  "email": "user@example.test"
}
```

Response:

```json
{
  "id": 1,
  "name": "Alex",
  "email": "user@example.test"
}
```

### `PATCH /api/users/:id`

Updates selected fields for a user.

```json
{
  "name": "Updated User"
}
```

Response:

```json
{
  "id": 1,
  "name": "Updated User",
  "email": "user@example.test"
}
```

### `DELETE /api/users/:id`

Deletes one user by ID.

```text
204 No Content
```

### Error Response JSON

```json
{
  "message": "User not found",
  "statusCode": 404,
  "code": "USER_NOT_FOUND"
}
```

### Pagination Query

```text
GET /api/users?page=1&size=10
```

Response:

```json
{
  "items": [
    {
      "id": 1,
      "name": "Alex",
      "email": "user@example.test"
    }
  ],
  "page": 1,
  "size": 10,
  "totalItems": 42
}
```
