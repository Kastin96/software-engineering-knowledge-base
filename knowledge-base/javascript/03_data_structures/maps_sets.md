# Maps and Sets

## Goal

Understand when to use `Map` and `Set` instead of plain objects or arrays.

## Why It Matters

Maps and sets are useful when you need key-value storage with flexible keys or
when you need a collection of unique values.

## Map

A `Map` stores key-value pairs.

```javascript
const userRoles = new Map();

userRoles.set("alex", "admin");
userRoles.set("sam", "editor");

console.log(userRoles.get("alex")); // admin
console.log(userRoles.has("sam")); // true
```

Unlike plain objects, map keys can be any value, including objects.

```javascript
const user = { id: 1 };
const permissions = new Map();

permissions.set(user, ["read", "write"]);

console.log(permissions.get(user)); // ["read", "write"]
```

## Iterating Over a Map

```javascript
const scores = new Map([
  ["Alex", 90],
  ["Sam", 85],
]);

for (const [name, score] of scores) {
  console.log(`${name}: ${score}`);
}
```

## Useful Map Methods

```javascript
const cache = new Map();

cache.set("user:1", { name: "Alex" });

console.log(cache.get("user:1"));
console.log(cache.has("user:1"));

cache.delete("user:1");

console.log(cache.size); // 0
```

## Set

A `Set` stores unique values.

```javascript
const tags = new Set();

tags.add("javascript");
tags.add("typescript");
tags.add("javascript");

console.log(tags); // Set { "javascript", "typescript" }
```

## Removing Duplicates

```javascript
const numbers = [1, 2, 2, 3, 3, 3];
const uniqueNumbers = [...new Set(numbers)];

console.log(uniqueNumbers); // [1, 2, 3]
```

## Checking Membership

```javascript
const allowedRoles = new Set(["admin", "editor"]);

console.log(allowedRoles.has("admin")); // true
console.log(allowedRoles.has("viewer")); // false
```

## Object vs Map

Use an object when you model a known shape.

```javascript
const user = {
  id: 1,
  name: "Alex",
};
```

Use a map when keys are dynamic or you need map-specific behavior.

```javascript
const requestCache = new Map();

requestCache.set("/api/users", [{ id: 1, name: "Alex" }]);
```

## Array vs Set

Use an array when order and duplicates matter.

```javascript
const history = ["home", "products", "products"];
```

Use a set when uniqueness matters.

```javascript
const visitedPages = new Set(["home", "products"]);
```

## Common Mistakes

- Trying to read map values with dot notation:

```javascript
const map = new Map();
map.set("name", "Alex");

console.log(map.get("name")); // correct
```

- Expecting a set to keep duplicates:

```javascript
const values = new Set([1, 1, 2]);

console.log(values.size); // 2
```

- Using a map for a fixed data shape:

```javascript
const user = {
  id: 1,
  name: "Alex",
};
```

For fixed fields, an object is usually clearer.

## Practice

1. Create a map of usernames to roles.
2. Create a set of unique tags from an array with duplicates.
3. Check whether a role exists in an allowed-role set.
4. Explain when an object is better than a map.

## Related Topics

- [Arrays](arrays.md)
- [Objects](objects.md)

