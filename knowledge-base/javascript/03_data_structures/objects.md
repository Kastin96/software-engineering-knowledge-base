# Objects

## Goal

Understand how to model named data with JavaScript objects.

## Why It Matters

Objects are used to describe users, products, settings, API responses, form
state, configuration, and almost every structured value in JavaScript.

## Explanation

An object stores data as key-value pairs.

```javascript
const user = {
  id: 1,
  name: "Alex",
  active: true,
};

console.log(user.name); // Alex
```

Keys are property names. Values can be any JavaScript value.

```javascript
const product = {
  name: "Keyboard",
  price: 99,
  tags: ["hardware", "office"],
  inStock: true,
};
```

## Reading Properties

Use dot notation for normal property names.

```javascript
const user = {
  name: "Alex",
};

console.log(user.name);
```

Use bracket notation when the property name is dynamic or not a valid identifier.

```javascript
const user = {
  "display-name": "Alex Smith",
};

console.log(user["display-name"]);
```

Dynamic property:

```javascript
const field = "email";

const user = {
  email: "alex@example.com",
};

console.log(user[field]);
```

## Updating Properties

```javascript
const user = {
  name: "Alex",
  active: true,
};

user.active = false;

console.log(user);
```

## Creating New Objects With Spread

Spread syntax creates a shallow copy with changes.

```javascript
const user = {
  name: "Alex",
  active: true,
};

const updatedUser = {
  ...user,
  active: false,
};

console.log(user); // original stays the same
console.log(updatedUser);
```

This pattern is common in React and state management.

## Methods

Objects can contain functions. These are called methods.

```javascript
const user = {
  name: "Alex",
  greet() {
    return `Hello, ${this.name}`;
  },
};

console.log(user.greet());
```

## Object Utilities

`Object.keys` returns property names.

```javascript
const user = {
  name: "Alex",
  active: true,
};

console.log(Object.keys(user)); // ["name", "active"]
```

`Object.values` returns values.

```javascript
console.log(Object.values(user)); // ["Alex", true]
```

`Object.entries` returns key-value pairs.

```javascript
console.log(Object.entries(user)); // [["name", "Alex"], ["active", true]]
```

## Optional Chaining

Optional chaining helps read nested values safely.

```javascript
const user = {
  profile: {
    city: "London",
  },
};

console.log(user.profile?.city); // London
console.log(user.settings?.theme); // undefined
```

## Common Mistakes

- Comparing objects by visible content:

```javascript
console.log({ id: 1 } === { id: 1 }); // false
```

Objects are compared by reference.

- Mutating an object when a copy would be safer:

```javascript
const settings = {
  theme: "light",
};

const nextSettings = {
  ...settings,
  theme: "dark",
};

console.log(nextSettings);
```

- Forgetting that spread is shallow:

```javascript
const user = {
  name: "Alex",
  address: {
    city: "London",
  },
};

const copy = { ...user };

copy.address.city = "Paris";

console.log(user.address.city); // Paris
```

Nested objects still share references.

## Practice

1. Create an object that describes a book.
2. Read one property with dot notation and one with bracket notation.
3. Create an updated copy of an object using spread.
4. Use `Object.keys`, `Object.values`, and `Object.entries`.

## Related Topics

- [Arrays](arrays.md)
- [Destructuring, Spread, and Rest](destructuring_spread_rest.md)
- [Prototypes](../02_core_concepts/prototypes.md)

