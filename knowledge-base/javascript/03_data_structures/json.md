# JSON

## Goal

Understand how JavaScript works with JSON data.

## Why It Matters

JSON is the most common data format for web APIs. Frontend and backend code
often sends, receives, stores, and parses JSON.

## Explanation

JSON means JavaScript Object Notation. It is a text format for structured data.

JSON looks similar to JavaScript objects, but it is a string format with stricter
rules.

```json
{
  "name": "Alex",
  "active": true,
  "skills": ["JavaScript", "TypeScript"]
}
```

JSON keys must use double quotes.

## JavaScript Object vs JSON

JavaScript object:

```javascript
const user = {
  name: "Alex",
  active: true,
};
```

JSON string:

```javascript
const userJson = '{"name":"Alex","active":true}';
```

## Convert Object to JSON

Use `JSON.stringify`.

```javascript
const user = {
  name: "Alex",
  active: true,
};

const json = JSON.stringify(user);

console.log(json); // {"name":"Alex","active":true}
```

Pretty formatting:

```javascript
const user = {
  name: "Alex",
  active: true,
};

console.log(JSON.stringify(user, null, 2));
```

## Convert JSON to Object

Use `JSON.parse`.

```javascript
const json = '{"name":"Alex","active":true}';
const user = JSON.parse(json);

console.log(user.name); // Alex
```

## Arrays in JSON

```javascript
const users = [
  { id: 1, name: "Alex" },
  { id: 2, name: "Sam" },
];

const json = JSON.stringify(users);
const parsedUsers = JSON.parse(json);

console.log(parsedUsers[0].name); // Alex
```

## Handling Invalid JSON

`JSON.parse` throws an error when the text is not valid JSON.

```javascript
const input = "{bad json}";

try {
  const data = JSON.parse(input);
  console.log(data);
} catch (error) {
  console.log("Invalid JSON");
}
```

## JSON Limitations

JSON cannot directly store functions, `undefined`, symbols, maps, sets, or date
objects as real JavaScript values.

```javascript
const data = {
  name: "Alex",
  sayHello() {
    return "Hello";
  },
  missing: undefined,
};

console.log(JSON.stringify(data)); // {"name":"Alex"}
```

Dates become strings.

```javascript
const data = {
  createdAt: new Date("2026-05-21T12:00:00Z"),
};

const json = JSON.stringify(data);

console.log(json);
```

## Common Mistakes

- Writing invalid JSON with single quotes:

```javascript
// Invalid JSON:
// {'name':'Alex'}
```

Correct JSON:

```json
{
  "name": "Alex"
}
```

- Forgetting that parsed JSON is normal JavaScript data:

```javascript
const user = JSON.parse('{"name":"Alex"}');

console.log(user.name);
```

- Expecting functions to survive `JSON.stringify`:

```javascript
const data = {
  run() {
    return "running";
  },
};

console.log(JSON.stringify(data)); // {}
```

## Practice

1. Convert a user object to JSON.
2. Parse a JSON string into an object.
3. Parse a JSON array of products.
4. Write a `try...catch` around `JSON.parse`.

## Related Topics

- [Objects](objects.md)
- [Arrays](arrays.md)
- [Dates](dates.md)

