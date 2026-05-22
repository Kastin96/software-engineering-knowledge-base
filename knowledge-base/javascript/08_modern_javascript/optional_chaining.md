# Optional Chaining

## Goal

Understand how to safely read nested properties that may not exist.

## Why It Matters

Real data is often incomplete. API responses, optional form fields, settings,
and user profiles may not contain every nested value. Optional chaining helps
avoid crashes when reading those values.

## Explanation

Optional chaining uses `?.`. If the value before `?.` is `null` or `undefined`,
JavaScript stops and returns `undefined`.

```javascript
const user = {
  profile: {
    city: "London",
  },
};

console.log(user.profile?.city); // London
console.log(user.settings?.theme); // undefined
```

Without optional chaining, nested access can throw.

```javascript
const user = {};

// TypeError
// console.log(user.profile.city);

console.log(user.profile?.city); // undefined
```

## Optional Chaining With Arrays

```javascript
const users = [{ name: "Alex" }];

console.log(users[0]?.name); // Alex
console.log(users[1]?.name); // undefined
```

## Optional Chaining With Functions

Use `?.()` when a function may not exist.

```javascript
const logger = {
  info(message) {
    console.log(message);
  },
};

logger.info?.("App started");
logger.debug?.("Debug message");
```

## Combining With Nullish Coalescing

Use optional chaining to read the value and nullish coalescing to provide a
default.

```javascript
const user = {};

const theme = user.settings?.theme ?? "light";

console.log(theme); // light
```

## Practical Example

```javascript
function getUserCity(apiResponse) {
  return apiResponse.data?.user?.address?.city ?? "Unknown city";
}

console.log(
  getUserCity({
    data: {
      user: {
        address: {
          city: "London",
        },
      },
    },
  }),
);

console.log(getUserCity({ data: {} }));
```

## Real Pain Points

- Optional chaining can hide a broken data contract if you use it everywhere.
  Use it for genuinely optional data, not for values that must exist.
- `?.` only protects the value immediately before it.

```javascript
const user = null;

console.log(user?.profile?.city); // safe
```

- Returning `undefined` may still need handling. Pair it with validation or a
  default value when the UI needs something concrete.

## Practice

1. Safely read `user.profile.email`.
2. Safely read the first item from an optional array.
3. Call an optional callback with `?.()`.
4. Combine optional chaining with `??` for a default value.

## Related Topics

- [Nullish Coalescing](nullish_coalescing.md)
- [Objects](../03_data_structures/objects.md)
- [Fetch in the Browser](../05_browser_javascript/fetch_in_browser.md)

