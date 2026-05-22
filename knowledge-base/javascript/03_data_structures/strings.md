# Strings

## Goal

Understand how to create, read, combine, and transform text in JavaScript.

## Why It Matters

Strings are used for names, messages, emails, URLs, IDs, logs, user input, and
API data.

## Explanation

A string is a text value.

```javascript
const name = "Alex";
const message = "Hello";

console.log(name);
console.log(message);
```

You can use double quotes, single quotes, or backticks.

```javascript
const first = "double quotes";
const second = 'single quotes';
const third = `backticks`;
```

Backticks create template literals.

```javascript
const userName = "Alex";
const greeting = `Hello, ${userName}`;

console.log(greeting); // Hello, Alex
```

## String Length and Indexes

```javascript
const language = "JavaScript";

console.log(language.length); // 10
console.log(language[0]); // J
```

String indexes start at `0`.

## Common Methods

`toUpperCase` and `toLowerCase`:

```javascript
const text = "JavaScript";

console.log(text.toUpperCase()); // JAVASCRIPT
console.log(text.toLowerCase()); // javascript
```

`trim` removes whitespace from the beginning and end.

```javascript
const input = "  alex@example.com  ";

console.log(input.trim()); // alex@example.com
```

`includes` checks whether text contains another string.

```javascript
const email = "alex@example.com";

console.log(email.includes("@")); // true
```

`startsWith` and `endsWith`:

```javascript
const fileName = "report.pdf";

console.log(fileName.endsWith(".pdf")); // true
```

`slice` extracts part of a string.

```javascript
const code = "JS-2026";

console.log(code.slice(0, 2)); // JS
```

`split` creates an array.

```javascript
const tags = "javascript,typescript,node";

console.log(tags.split(",")); // ["javascript", "typescript", "node"]
```

`replace` replaces matching text.

```javascript
const title = "JavaScript basics";

console.log(title.replace("basics", "guide"));
```

## Strings Are Immutable

String methods return new strings. They do not change the original string.

```javascript
const name = "alex";
const upperName = name.toUpperCase();

console.log(name); // alex
console.log(upperName); // ALEX
```

## Practical Example

Normalize user input.

```javascript
function normalizeEmail(email) {
  return email.trim().toLowerCase();
}

console.log(normalizeEmail("  ALEX@EXAMPLE.COM  "));
```

## Common Mistakes

- Forgetting that string methods return a new value:

```javascript
const text = "hello";
const upperText = text.toUpperCase();

console.log(upperText); // HELLO
```

- Using `+` when a template literal is clearer:

```javascript
const name = "Alex";
const score = 90;

console.log(`${name} scored ${score}`);
```

- Forgetting case sensitivity:

```javascript
const role = "Admin";

console.log(role === "admin"); // false
console.log(role.toLowerCase() === "admin"); // true
```

## Practice

1. Create a function that trims and lowercases an email.
2. Check if a string contains a word.
3. Split a comma-separated list into an array.
4. Create a message with a template literal.

## Related Topics

- [Arrays](arrays.md)
- [JSON](json.md)

