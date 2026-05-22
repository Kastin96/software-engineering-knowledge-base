# Data Types

## Goal

Understand the basic types of values in JavaScript.

## Why It Matters

Every value in JavaScript has a type. Knowing data types helps you avoid bugs,
write correct conditions, and understand how functions should behave.

## Explanation

JavaScript has primitive types and reference types.

Primitive values are simple values:

- `string`
- `number`
- `boolean`
- `null`
- `undefined`
- `bigint`
- `symbol`

Reference values include objects, arrays, and functions.

## Primitive Types

String:

```javascript
const name = "Alex";
const message = "Hello";

console.log(name);
console.log(message);
```

Number:

```javascript
const price = 99.99;
const quantity = 2;

console.log(price * quantity);
```

Boolean:

```javascript
const isActive = true;
const isDeleted = false;

console.log(isActive);
console.log(isDeleted);
```

`null` means an intentional empty value.

```javascript
const selectedUser = null;
console.log(selectedUser);
```

`undefined` usually means a value was not assigned.

```javascript
let currentUser;
console.log(currentUser); // undefined
```

BigInt is used for very large integers.

```javascript
const largeNumber = 9007199254740993n;
console.log(largeNumber);
```

Symbol creates a unique value.

```javascript
const id = Symbol("id");
console.log(id);
```

## Reference Types

Object:

```javascript
const user = {
  id: 1,
  name: "Alex",
  active: true,
};

console.log(user.name);
```

Array:

```javascript
const languages = ["JavaScript", "TypeScript", "SQL"];

console.log(languages[0]); // JavaScript
```

Function:

```javascript
function sayHello() {
  return "Hello";
}

console.log(sayHello());
```

## Checking Types

Use `typeof` to check many primitive types.

```javascript
console.log(typeof "Hello"); // string
console.log(typeof 42); // number
console.log(typeof true); // boolean
console.log(typeof undefined); // undefined
```

Important detail:

```javascript
console.log(typeof null); // object
```

This is a known JavaScript behavior. For `null`, check directly:

```javascript
const value = null;

if (value === null) {
  console.log("The value is null");
}
```

Use `Array.isArray()` for arrays.

```javascript
const items = [];

console.log(Array.isArray(items)); // true
```

## Common Mistakes

- Confusing `null` and `undefined`:

```javascript
let notAssigned;
const intentionallyEmpty = null;

console.log(notAssigned); // undefined
console.log(intentionallyEmpty); // null
```

- Expecting arrays to return `"array"` from `typeof`:

```javascript
const list = [1, 2, 3];

console.log(typeof list); // object
console.log(Array.isArray(list)); // true
```

- Comparing objects by content:

```javascript
console.log({ id: 1 } === { id: 1 }); // false
```

Objects are compared by reference, not by their visible content.

## Practice

1. Create one value for each primitive type.
2. Create an object that describes a book.
3. Create an array of three skills.
4. Use `typeof` and `Array.isArray()` to inspect your values.

## Related Topics

- [Variables](variables.md)
- [Operators](operators.md)
- [Functions](functions.md)

