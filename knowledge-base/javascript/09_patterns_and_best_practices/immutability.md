# Immutability

## Goal

Understand how to update data without changing the original value.

## Why It Matters

Immutability makes state changes easier to track. It is especially useful in UI
code, reducers, caching, tests, and any logic where accidental changes create
hard-to-find bugs.

## Explanation

Mutable code changes an existing value.

```javascript
const user = {
  name: "Alex",
  active: true,
};

user.active = false;
```

Immutable-style code creates a new value with the change.

```javascript
const user = {
  name: "Alex",
  active: true,
};

const updatedUser = {
  ...user,
  active: false,
};

console.log(user.active); // true
console.log(updatedUser.active); // false
```

## Updating Arrays

Add an item:

```javascript
const tasks = ["learn scope", "learn arrays"];
const nextTasks = [...tasks, "learn immutability"];

console.log(nextTasks);
```

Remove an item:

```javascript
const tasks = [
  { id: 1, title: "Learn JS" },
  { id: 2, title: "Practice" },
];

const nextTasks = tasks.filter((task) => task.id !== 1);

console.log(nextTasks);
```

Update an item:

```javascript
const users = [
  { id: 1, name: "Alex", active: true },
  { id: 2, name: "Sam", active: true },
];

const nextUsers = users.map((user) => {
  if (user.id !== 2) {
    return user;
  }

  return {
    ...user,
    active: false,
  };
});

console.log(nextUsers);
```

## Updating Nested Objects

Object spread is shallow, so nested objects need their own copies.

```javascript
const user = {
  name: "Alex",
  address: {
    city: "London",
  },
};

const updatedUser = {
  ...user,
  address: {
    ...user.address,
    city: "Paris",
  },
};

console.log(user.address.city); // London
console.log(updatedUser.address.city); // Paris
```

## Object.freeze

`Object.freeze` prevents direct changes to an object at runtime.

```javascript
const settings = Object.freeze({
  theme: "dark",
});

// In strict mode, this throws.
// settings.theme = "light";
```

It is shallow, so nested objects are not automatically frozen.

## Real Pain Points

- Shallow copies do not protect nested values.
- Mutating function arguments can surprise callers.
- Immutable updates can become verbose for deeply nested data. In that case,
  consider flatter data shapes or a helper library in real projects.
- Immutability is a tool, not a religion. Local mutation inside a small function
  can be fine if it does not leak outside.

```javascript
function buildUserNames(users) {
  const names = [];

  for (const user of users) {
    names.push(user.name);
  }

  return names;
}
```

The local `names` array is mutated internally, but callers only receive the
final result.

## Practice

1. Add an item to an array without mutating the original array.
2. Update one object inside an array by id.
3. Update a nested object using nested spread.
4. Refactor a function that mutates its argument.

## Related Topics

- [Arrays](../03_data_structures/arrays.md)
- [Objects](../03_data_structures/objects.md)
- [Pure Functions](pure_functions.md)

