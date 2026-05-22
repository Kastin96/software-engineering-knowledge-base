# Debugging

## Goal

Learn a practical workflow for finding and fixing bugs.

## Why It Matters

Debugging is not guessing. A good debugging process helps you move from
"something is broken" to a specific cause you can fix and verify.

## Debugging Mindset

Start with a clear question:

```text
What did I expect to happen?
What actually happened?
Where is the first place those two things differ?
```

Then narrow the problem step by step.

## Basic Workflow

1. Reproduce the issue.
2. Read the error message and stack trace.
3. Identify the smallest failing path.
4. Inspect inputs and outputs.
5. Make one small change.
6. Verify the fix.
7. Remove temporary debug code.

## Reproduce the Issue

Create a small example that shows the bug.

```javascript
function calculateDiscount(price, percent) {
  return price * percent;
}

console.log(calculateDiscount(100, 20)); // expected 20, actual 2000
```

The bug is easier to see when the input is small and specific.

## Inspect Inputs and Outputs

```javascript
function calculateDiscount(price, percent) {
  console.log({ price, percent });
  return price * (percent / 100);
}

console.log(calculateDiscount(100, 20)); // 20
```

## Use Breakpoints

A breakpoint pauses code so you can inspect values at that exact line.

```javascript
function createUser(input) {
  const email = input.email.trim().toLowerCase();

  return {
    email,
  };
}
```

Set a breakpoint on the line that creates `email`, then inspect `input`.

## Debug the Boundary First

Many bugs start at boundaries:

- user input;
- API response;
- environment variables;
- parsed JSON;
- command-line arguments;
- database result;
- local storage value.

Example:

```javascript
function normalizeUser(apiUser) {
  return {
    id: apiUser.id,
    name: apiUser.name.trim(),
  };
}
```

Before changing this function, inspect the actual `apiUser` value.

## Add a Failing Test or Small Script

For repeatable bugs, write a small check.

```javascript
function toPercent(value) {
  return `${value * 100}%`;
}

console.log(toPercent(0.2)); // 20%
```

This makes it easier to know when the bug is fixed.

## Real Pain Points

- Debugging from assumptions wastes time. Inspect the actual value.
- Changing many things at once makes it hard to know what fixed the issue.
- Logs without labels become confusing quickly.

```javascript
console.log("user before validation", user);
console.log("validation errors", errors);
```

- Fixing the symptom without understanding the cause can create a second bug.

## Practice

1. Take a function with a wrong result and write the expected result.
2. Add labeled logs for the input and output.
3. Use a breakpoint to inspect a value before it changes.
4. Remove temporary logs after the bug is fixed.

## Related Topics

- [Console](console.md)
- [DevTools](devtools.md)
- [Errors](errors.md)

