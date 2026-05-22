# Operators

## Goal

Understand how to calculate, compare, assign, and combine values.

## Why It Matters

Operators are used everywhere: calculations, conditions, validation, updates,
and business rules.

## Explanation

An operator performs an action on one or more values.

```javascript
const total = 10 + 5;
console.log(total); // 15
```

## Arithmetic Operators

```javascript
console.log(10 + 3); // 13
console.log(10 - 3); // 7
console.log(10 * 3); // 30
console.log(10 / 3); // 3.3333333333333335
console.log(10 % 3); // 1
console.log(2 ** 3); // 8
```

Example:

```javascript
const price = 50;
const quantity = 3;
const total = price * quantity;

console.log(total); // 150
```

## Assignment Operators

```javascript
let count = 10;

count += 5; // same as count = count + 5
count -= 2; // same as count = count - 2
count *= 3; // same as count = count * 3
count /= 2; // same as count = count / 2

console.log(count);
```

## Comparison Operators

```javascript
console.log(5 > 3); // true
console.log(5 < 3); // false
console.log(5 >= 5); // true
console.log(5 <= 4); // false
```

Prefer strict equality:

```javascript
console.log(5 === 5); // true
console.log(5 === "5"); // false
console.log(5 !== "5"); // true
```

Avoid loose equality in normal code:

```javascript
console.log(5 == "5"); // true
```

## Logical Operators

`&&` means "and".

```javascript
const hasEmail = true;
const hasPassword = true;

console.log(hasEmail && hasPassword); // true
```

`||` means "or".

```javascript
const isAdmin = false;
const isOwner = true;

console.log(isAdmin || isOwner); // true
```

`!` means "not".

```javascript
const isActive = false;

console.log(!isActive); // true
```

## String Operators

The `+` operator can join strings.

```javascript
const firstName = "Alex";
const lastName = "Smith";

console.log(firstName + " " + lastName);
```

Template literals are usually easier to read.

```javascript
const firstName = "Alex";
const lastName = "Smith";

console.log(`${firstName} ${lastName}`);
```

## Nullish Coalescing

`??` returns the right value only when the left value is `null` or `undefined`.

```javascript
const savedTheme = null;
const theme = savedTheme ?? "light";

console.log(theme); // light
```

This is useful for default values.

```javascript
const pageSizeFromSettings = undefined;
const pageSize = pageSizeFromSettings ?? 20;

console.log(pageSize); // 20
```

## Common Mistakes

- Using `==` instead of `===`:

```javascript
console.log(false == 0); // true
console.log(false === 0); // false
```

- Using `||` when `0` is a valid value:

```javascript
const userLimit = 0;

const limitWithOr = userLimit || 10;
const limitWithNullish = userLimit ?? 10;

console.log(limitWithOr); // 10
console.log(limitWithNullish); // 0
```

- Forgetting operator precedence:

```javascript
const result = 2 + 3 * 4;
console.log(result); // 14

const clearerResult = (2 + 3) * 4;
console.log(clearerResult); // 20
```

## Practice

1. Calculate the total price for `price` and `quantity`.
2. Check whether a user is both active and verified.
3. Use `??` to create a default page size.
4. Compare two values using `===` and explain the result.

## Related Topics

- [Variables](variables.md)
- [Data Types](data_types.md)
- [Control Flow](control_flow.md)

