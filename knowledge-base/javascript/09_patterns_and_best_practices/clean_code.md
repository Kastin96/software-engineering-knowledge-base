# Clean Code

## Goal

Write JavaScript that is easy to read, understand, and change.

## Why It Matters

Most code is read more often than it is written. Clean code reduces bugs,
improves reviews, makes onboarding easier, and helps future you understand what
past you meant.

## Clear Names

Names should explain purpose.

```javascript
const userEmail = "alex@example.com";
const activeUsers = users.filter((user) => user.active);
```

Avoid names that only make sense in your head.

```javascript
const x = "alex@example.com";
const arr = users.filter((u) => u.active);
```

Short names are fine in tiny scopes.

```javascript
const total = prices.reduce((sum, price) => sum + price, 0);
```

## Small Functions

A function should usually do one clear job.

```javascript
function isValidEmail(email) {
  return email.includes("@");
}

function normalizeEmail(email) {
  return email.trim().toLowerCase();
}

function createUser(input) {
  return {
    name: input.name.trim(),
    email: normalizeEmail(input.email),
  };
}
```

## Early Returns

Early returns can make validation and branching easier to read.

```javascript
function getAccessMessage(user) {
  if (!user) {
    return "User is required";
  }

  if (!user.active) {
    return "User is inactive";
  }

  return "Access granted";
}
```

## Keep Levels of Detail Separate

High-level functions should read like a story.

```javascript
async function handleSignup(form) {
  const input = readSignupForm(form);
  const errors = validateSignupInput(input);

  if (Object.keys(errors).length > 0) {
    showValidationErrors(errors);
    return;
  }

  await submitSignup(input);
  showSuccessMessage();
}
```

Each helper can contain the lower-level details.

## Prefer Explicit Data Shapes

```javascript
const user = {
  id: 1,
  name: "Alex",
  email: "alex@example.com",
  active: true,
};
```

This is easier to understand than passing loosely related values around.

## Comments

Good comments explain why, not what obvious code already says.

```javascript
// The API expects cents, so convert dollars before sending.
const priceInCents = priceInDollars * 100;
```

Avoid comments that repeat the code.

```javascript
// Increment count by one.
count += 1;
```

## Real Pain Points

- A function with many responsibilities is hard to test and easy to break.
- Clever one-liners often become expensive when someone has to debug them.
- Comments that explain old behavior can become misleading after code changes.
- Inconsistent names make related concepts look unrelated.

## Practice

1. Rename vague variables in a small function.
2. Split a large function into three smaller functions.
3. Replace a nested `if` chain with early returns.
4. Add one useful comment that explains why a decision exists.

## Related Topics

- [Pure Functions](pure_functions.md)
- [Common Antipatterns](common_antipatterns.md)
- [Debugging](../07_error_handling_debugging/debugging.md)

