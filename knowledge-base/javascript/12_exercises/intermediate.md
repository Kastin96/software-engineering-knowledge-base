# Intermediate Exercises

## 1. Group Users by Role

Write a function `groupUsersByRole(users)` that returns an object where each key
is a role and each value is an array of users with that role.

```javascript
const users = [
  { id: 1, name: "Alex", role: "admin" },
  { id: 2, name: "Sam", role: "editor" },
  { id: 3, name: "Jordan", role: "admin" },
];

groupUsersByRole(users);
// {
//   admin: [Alex, Jordan],
//   editor: [Sam]
// }
```

## 2. Normalize API Users

Write a function `normalizeApiUsers(apiUsers)` that converts API field names to
UI field names.

```javascript
const apiUsers = [
  {
    user_id: 1,
    first_name: "Alex",
    last_name: "Smith",
    email_address: " ALEX@EXAMPLE.COM ",
    is_active: true,
  },
];
```

Expected item shape:

```javascript
{
  id: "1",
  fullName: "Alex Smith",
  email: "alex@example.com",
  active: true,
}
```

## 3. Validate Product

Write a function `validateProduct(input)` that returns an error object.

Rules:

- `name` is required;
- `price` must be a number greater than `0`;
- `sku` must be uppercase letters, numbers, or dashes.

```javascript
validateProduct({
  name: "",
  price: -10,
  sku: "bad sku",
});
// {
//   name: "Name is required",
//   price: "Price must be greater than 0",
//   sku: "SKU format is invalid"
// }
```

## 4. Update User in List

Write a function `updateUserById(users, id, updates)` that returns a new array
with one updated user. Do not mutate the original array or user object.

```javascript
updateUserById(users, 2, { active: true });
```

## 5. Build Query String

Write a function `buildQueryString(params)` that:

- skips `null`, `undefined`, and empty string values;
- includes `0` and `false`;
- returns a query string without a leading `?`.

```javascript
buildQueryString({
  page: 1,
  search: "keyboard",
  archived: false,
  empty: "",
});
// "page=1&search=keyboard&archived=false"
```

## 6. Create Pagination Metadata

Write a function `createPaginationMeta({ page, pageSize, totalItems })`.

Expected output:

```javascript
{
  page: 2,
  pageSize: 10,
  totalItems: 42,
  totalPages: 5,
  hasPreviousPage: true,
  hasNextPage: true,
}
```

Clamp `page` so it cannot be lower than `1` or higher than `totalPages`.

## 7. Retry Async Operation

Write an async function `retry(operation, attempts)` that:

- runs `operation`;
- returns the result if it succeeds;
- retries after failure;
- throws the last error when all attempts fail.

```javascript
const result = await retry(() => fetchJson("/api/users"), 3);
```

## 8. Convert List to Lookup

Write a function `createLookup(items, key)` that returns an object indexed by a
field.

```javascript
createLookup(
  [
    { id: "u1", name: "Alex" },
    { id: "u2", name: "Sam" },
  ],
  "id",
);
// {
//   u1: { id: "u1", name: "Alex" },
//   u2: { id: "u2", name: "Sam" }
// }
```

