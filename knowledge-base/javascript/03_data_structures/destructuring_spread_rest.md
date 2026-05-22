# Destructuring, Spread, and Rest

## Goal

Understand how to read, copy, combine, and collect values with modern JavaScript
syntax.

## Why It Matters

Destructuring, spread, and rest syntax are used heavily in modern JavaScript,
React, Node.js, API handling, and everyday data transformations.

## Destructuring Objects

Destructuring lets you extract values from objects.

```javascript
const user = {
  name: "Alex",
  email: "alex@example.com",
};

const { name, email } = user;

console.log(name);
console.log(email);
```

You can rename variables.

```javascript
const user = {
  name: "Alex",
};

const { name: userName } = user;

console.log(userName);
```

You can use default values.

```javascript
const settings = {
  theme: "dark",
};

const { theme, language = "en" } = settings;

console.log(theme); // dark
console.log(language); // en
```

## Destructuring Arrays

```javascript
const colors = ["red", "green", "blue"];

const [firstColor, secondColor] = colors;

console.log(firstColor); // red
console.log(secondColor); // green
```

Skip values with empty positions.

```javascript
const values = ["first", "second", "third"];

const [first, , third] = values;

console.log(first);
console.log(third);
```

## Spread With Objects

Spread copies object properties into a new object.

```javascript
const user = {
  name: "Alex",
  active: true,
};

const updatedUser = {
  ...user,
  active: false,
};

console.log(updatedUser);
```

Later properties overwrite earlier properties.

```javascript
const result = {
  name: "Alex",
  name: "Sam",
};

console.log(result.name); // Sam
```

## Spread With Arrays

```javascript
const first = [1, 2];
const second = [3, 4];

const combined = [...first, ...second];

console.log(combined); // [1, 2, 3, 4]
```

Add an item without changing the original array.

```javascript
const tasks = ["learn scope", "learn closures"];
const nextTasks = [...tasks, "learn arrays"];

console.log(tasks);
console.log(nextTasks);
```

## Rest With Objects

Rest collects remaining properties.

```javascript
const user = {
  id: 1,
  name: "Alex",
  email: "alex@example.com",
};

const { id, ...profile } = user;

console.log(id);
console.log(profile); // { name: "Alex", email: "alex@example.com" }
```

## Rest With Arrays

```javascript
const numbers = [1, 2, 3, 4];

const [first, ...rest] = numbers;

console.log(first); // 1
console.log(rest); // [2, 3, 4]
```

## Rest Parameters

Rest parameters collect function arguments into an array.

```javascript
function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(1, 2, 3)); // 6
```

## Common Mistakes

- Confusing spread and rest:

```javascript
const numbers = [1, 2, 3];

const copy = [...numbers]; // spread expands values
const [first, ...others] = numbers; // rest collects values

console.log(copy);
console.log(first);
console.log(others);
```

- Forgetting that object spread is shallow:

```javascript
const user = {
  profile: {
    city: "London",
  },
};

const copy = { ...user };

copy.profile.city = "Paris";

console.log(user.profile.city); // Paris
```

- Using destructuring when simple property access is clearer:

```javascript
const user = {
  name: "Alex",
};

console.log(user.name);
```

Destructuring is useful, but clarity matters more.

## Practice

1. Destructure `name` and `email` from a user object.
2. Destructure the first two values from an array.
3. Create an updated object using spread.
4. Write a function that accepts any number of numbers with rest parameters.

## Related Topics

- [Arrays](arrays.md)
- [Objects](objects.md)
- [Functions](../01_language_basics/functions.md)
