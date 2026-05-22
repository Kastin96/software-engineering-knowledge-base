# Beginner Exercises

## 1. Normalize Email

Write a function `normalizeEmail(email)` that:

- trims spaces;
- converts the email to lowercase.

```javascript
normalizeEmail("  ALEX@EXAMPLE.COM  ");
// "alex@example.com"
```

## 2. Calculate Cart Total

Write a function `calculateCartTotal(items)` that returns the total price.

```javascript
const items = [
  { name: "Keyboard", price: 99, quantity: 1 },
  { name: "Mouse", price: 40, quantity: 2 },
];

calculateCartTotal(items);
// 179
```

## 3. Get Active Users

Write a function `getActiveUsers(users)` that returns only users with
`active: true`.

```javascript
const users = [
  { id: 1, name: "Alex", active: true },
  { id: 2, name: "Sam", active: false },
  { id: 3, name: "Jordan", active: true },
];

getActiveUsers(users);
// Alex and Jordan
```

## 4. Format User Label

Write a function `formatUserLabel(user)` that returns:

```text
Alex Smith <alex@example.com>
```

Input:

```javascript
const user = {
  firstName: "Alex",
  lastName: "Smith",
  email: "alex@example.com",
};
```

## 5. Count Words

Write a function `countWords(text)` that counts words in a string.

```javascript
countWords("JavaScript is fun");
// 3

countWords("  JavaScript   is   practical  ");
// 3
```

## 6. Create Slug

Write a function `createSlug(title)` that:

- trims the title;
- lowercases it;
- replaces groups of spaces with `-`;
- removes leading and trailing dashes.

```javascript
createSlug("  JavaScript Practical Examples  ");
// "javascript-practical-examples"
```

## 7. Find User by Id

Write a function `findUserById(users, id)` that returns the matching user or
`null`.

```javascript
findUserById(users, 2);
// { id: 2, name: "Sam", active: false }

findUserById(users, 99);
// null
```

## 8. Build Initials

Write a function `getInitials(fullName)` that returns uppercase initials.

```javascript
getInitials("Alex Smith");
// "AS"

getInitials("  sam   green  ");
// "SG"
```

## 9. Safe Number Parse

Write a function `parsePrice(value)` that:

- converts a string to a number;
- returns `0` when the value is not a valid number.

```javascript
parsePrice("19.99");
// 19.99

parsePrice("abc");
// 0
```

## 10. Has Permission

Write a function `hasPermission(user, permission)` that checks whether a user
has a permission in `user.permissions`.

```javascript
const user = {
  name: "Alex",
  permissions: ["read", "write"],
};

hasPermission(user, "write");
// true

hasPermission(user, "delete");
// false
```

