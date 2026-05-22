# Control Flow

## Goal

Understand how to make decisions and repeat actions in JavaScript.

## Why It Matters

Real programs need logic. They must choose what to do based on data and often
repeat the same action for many values.

## Explanation

Control flow decides which code runs, when it runs, and how many times it runs.

## If Statements

Use `if` when code should run only when a condition is true.

```javascript
const age = 20;

if (age >= 18) {
  console.log("Adult user");
}
```

Use `else` for the alternative path.

```javascript
const isLoggedIn = false;

if (isLoggedIn) {
  console.log("Show dashboard");
} else {
  console.log("Show login page");
}
```

Use `else if` for multiple conditions.

```javascript
const score = 85;

if (score >= 90) {
  console.log("Excellent");
} else if (score >= 70) {
  console.log("Good");
} else {
  console.log("Needs practice");
}
```

## Switch Statements

Use `switch` when one value can match several known cases.

```javascript
const role = "admin";

switch (role) {
  case "admin":
    console.log("Full access");
    break;
  case "editor":
    console.log("Can edit content");
    break;
  case "viewer":
    console.log("Can view content");
    break;
  default:
    console.log("Unknown role");
}
```

## For Loops

Use a `for` loop when you know how many times to repeat something.

```javascript
for (let index = 0; index < 3; index += 1) {
  console.log(index);
}
```

Loop over an array:

```javascript
const languages = ["JavaScript", "TypeScript", "SQL"];

for (let index = 0; index < languages.length; index += 1) {
  console.log(languages[index]);
}
```

## For...of Loops

Use `for...of` to read values from an array.

```javascript
const users = ["Alex", "Sam", "Jordan"];

for (const user of users) {
  console.log(user);
}
```

## While Loops

Use `while` when the loop should continue while a condition is true.

```javascript
let count = 0;

while (count < 3) {
  console.log(count);
  count += 1;
}
```

## Break and Continue

`break` stops a loop.

```javascript
const numbers = [1, 2, 3, 4, 5];

for (const number of numbers) {
  if (number === 3) {
    break;
  }

  console.log(number);
}
```

`continue` skips the current loop step.

```javascript
const numbers = [1, 2, 3, 4, 5];

for (const number of numbers) {
  if (number % 2 === 0) {
    continue;
  }

  console.log(number);
}
```

## Common Mistakes

- Forgetting `break` in a `switch`:

```javascript
const status = "pending";

switch (status) {
  case "pending":
    console.log("Waiting");
    break;
  case "done":
    console.log("Complete");
    break;
}
```

- Creating an infinite loop:

```javascript
let count = 0;

while (count < 3) {
  console.log(count);
  count += 1;
}
```

- Using a complex condition that is hard to read:

```javascript
const user = {
  active: true,
  verified: true,
};

const canAccessDashboard = user.active && user.verified;

if (canAccessDashboard) {
  console.log("Access granted");
}
```

## Practice

1. Write an `if` statement that checks if a number is positive.
2. Write a `switch` for user roles: `admin`, `editor`, and `viewer`.
3. Loop over an array of names and print each name.
4. Print only odd numbers from an array.

## Related Topics

- [Operators](operators.md)
- [Functions](functions.md)

