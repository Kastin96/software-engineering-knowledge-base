# Dates

## Goal

Understand how to create and use dates in JavaScript.

## Why It Matters

Dates appear in logs, schedules, created-at fields, updated-at fields, reports,
expiration times, and API data.

## Explanation

JavaScript has a built-in `Date` object.

```javascript
const now = new Date();

console.log(now);
```

Create a date from an ISO string.

```javascript
const createdAt = new Date("2026-05-21T12:00:00Z");

console.log(createdAt);
```

ISO strings are a common format for API data.

## Reading Date Parts

```javascript
const date = new Date("2026-05-21T12:00:00Z");

console.log(date.getFullYear());
console.log(date.getMonth()); // 0-based: January is 0
console.log(date.getDate());
```

Important detail: `getMonth()` starts at `0`.

```javascript
const date = new Date("2026-01-15T00:00:00Z");

console.log(date.getMonth()); // 0
```

## Formatting Dates

Use `Intl.DateTimeFormat` for readable formatting.

```javascript
const date = new Date("2026-05-21T12:00:00Z");

const formatter = new Intl.DateTimeFormat("en-US", {
  year: "numeric",
  month: "long",
  day: "numeric",
});

console.log(formatter.format(date)); // May 21, 2026
```

## Timestamps

A timestamp is a number that represents time in milliseconds since January 1,
1970 UTC.

```javascript
const now = Date.now();

console.log(now);
```

You can compare dates with timestamps.

```javascript
const start = new Date("2026-05-21T10:00:00Z");
const end = new Date("2026-05-21T12:00:00Z");

console.log(end.getTime() > start.getTime()); // true
```

## Adding Time

```javascript
const oneDay = 24 * 60 * 60 * 1000;
const today = new Date("2026-05-21T00:00:00Z");
const tomorrow = new Date(today.getTime() + oneDay);

console.log(tomorrow.toISOString());
```

## JSON and Dates

Dates are usually sent through APIs as strings.

```javascript
const user = {
  name: "Alex",
  createdAt: new Date("2026-05-21T12:00:00Z").toISOString(),
};

console.log(JSON.stringify(user));
```

When reading the data back, convert the string to a `Date` if you need date
methods.

```javascript
const data = {
  createdAt: "2026-05-21T12:00:00.000Z",
};

const createdAt = new Date(data.createdAt);

console.log(createdAt.getFullYear());
```

## Common Mistakes

- Forgetting that months are zero-based:

```javascript
const date = new Date(2026, 0, 15);

console.log(date); // January 15, 2026
```

- Treating every date string as safe:

```javascript
const date = new Date("not a real date");

console.log(Number.isNaN(date.getTime())); // true
```

- Ignoring time zones:

Dates can display differently depending on local time zone. Use ISO strings and
clear formatting rules when sharing dates between systems.

## Practice

1. Create a date for today.
2. Create a date from an ISO string.
3. Format a date with `Intl.DateTimeFormat`.
4. Compare two dates with `getTime()`.

## Related Topics

- [JSON](json.md)
- [Strings](strings.md)

