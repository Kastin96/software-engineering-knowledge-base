# Arrays

## Goal

Understand how to store ordered lists of values and work with array methods.

## Why It Matters

Arrays are used for lists of users, products, messages, search results, form
errors, menu items, and many other collections in real projects.

## Explanation

An array is an ordered list of values.

```javascript
const languages = ["JavaScript", "TypeScript", "SQL"];

console.log(languages[0]); // JavaScript
console.log(languages[1]); // TypeScript
```

Array indexes start at `0`.

```javascript
const colors = ["red", "green", "blue"];

console.log(colors[0]); // red
console.log(colors.length); // 3
```

## Adding and Removing Items

`push` adds an item to the end.

```javascript
const tasks = ["learn variables", "learn functions"];

tasks.push("learn arrays");

console.log(tasks);
```

`pop` removes the last item.

```javascript
const tasks = ["one", "two", "three"];

const lastTask = tasks.pop();

console.log(lastTask); // three
console.log(tasks); // ["one", "two"]
```

`unshift` adds to the beginning and `shift` removes from the beginning.

```javascript
const numbers = [2, 3];

numbers.unshift(1);

console.log(numbers); // [1, 2, 3]

const first = numbers.shift();

console.log(first); // 1
```

## Reading Items With Loops

```javascript
const users = ["Alex", "Sam", "Jordan"];

for (const user of users) {
  console.log(user);
}
```

## Common Array Methods

`map` creates a new array by transforming each item.

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map((number) => number * 2);

console.log(doubled); // [2, 4, 6]
```

`filter` creates a new array with only matching items.

```javascript
const users = [
  { name: "Alex", active: true },
  { name: "Sam", active: false },
];

const activeUsers = users.filter((user) => user.active);

console.log(activeUsers);
```

`find` returns the first matching item.

```javascript
const users = [
  { id: 1, name: "Alex" },
  { id: 2, name: "Sam" },
];

const user = users.find((item) => item.id === 2);

console.log(user); // { id: 2, name: "Sam" }
```

`some` checks whether at least one item matches.

```javascript
const numbers = [1, 3, 5, 8];

console.log(numbers.some((number) => number % 2 === 0)); // true
```

`every` checks whether all items match.

```javascript
const scores = [80, 90, 100];

console.log(scores.every((score) => score >= 70)); // true
```

`reduce` combines items into one result.

```javascript
const prices = [10, 20, 30];

const total = prices.reduce((sum, price) => sum + price, 0);

console.log(total); // 60
```

## Copying Arrays

Use spread syntax to create a shallow copy.

```javascript
const original = ["a", "b"];
const copy = [...original];

copy.push("c");

console.log(original); // ["a", "b"]
console.log(copy); // ["a", "b", "c"]
```

## Common Mistakes

- Expecting `map` to change the original array:

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map((number) => number * 2);

console.log(numbers); // [1, 2, 3]
console.log(doubled); // [2, 4, 6]
```

- Using `forEach` when you need a returned array:

```javascript
const names = ["Alex", "Sam"];
const upperNames = names.map((name) => name.toUpperCase());

console.log(upperNames);
```

- Forgetting that array indexes start at `0`:

```javascript
const items = ["first", "second"];

console.log(items[0]); // first
```

## Practice

1. Create an array of five numbers and calculate their total.
2. Filter a list of users to only active users.
3. Find a product by `id`.
4. Create a new array with all names uppercased.

## Related Topics

- [Objects](objects.md)
- [Destructuring, Spread, and Rest](destructuring_spread_rest.md)

