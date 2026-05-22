# Storage

## Goal

Understand how to store small amounts of data in the browser.

## Why It Matters

Browser storage is useful for preferences, draft data, simple cached values,
feature flags, and small UI state that should survive a page refresh.

## localStorage

`localStorage` stores string values with no automatic expiration.

```javascript
localStorage.setItem("theme", "dark");

const theme = localStorage.getItem("theme");

console.log(theme); // dark
```

Remove one value:

```javascript
localStorage.removeItem("theme");
```

Clear all values for the current origin:

```javascript
localStorage.clear();
```

## sessionStorage

`sessionStorage` works like `localStorage`, but it is scoped to the browser tab
session.

```javascript
sessionStorage.setItem("step", "2");

console.log(sessionStorage.getItem("step"));
```

## Storing Objects

Storage stores strings, so objects need JSON.

```javascript
const userSettings = {
  theme: "dark",
  compactMode: true,
};

localStorage.setItem("settings", JSON.stringify(userSettings));
```

Read the object back:

```javascript
const savedSettings = localStorage.getItem("settings");

const settings = savedSettings ? JSON.parse(savedSettings) : null;

console.log(settings);
```

## Safe Parse Helper

Stored values can be missing or invalid.

```javascript
function readJsonFromStorage(key, fallback) {
  const value = localStorage.getItem(key);

  if (!value) {
    return fallback;
  }

  try {
    return JSON.parse(value);
  } catch {
    return fallback;
  }
}

const settings = readJsonFromStorage("settings", {
  theme: "light",
});

console.log(settings);
```

## Storage Event

The `storage` event fires in other tabs from the same origin when storage
changes.

```javascript
window.addEventListener("storage", (event) => {
  if (event.key === "theme") {
    console.log("Theme changed in another tab:", event.newValue);
  }
});
```

## Real Pain Points

- Browser storage is not secure storage. Do not store passwords, tokens, or
  sensitive personal data there unless you fully understand the security model.
- Storage values are strings. Always serialize and parse structured data.
- Storage can be unavailable or limited in some privacy modes or environments,
  so critical app logic should not depend on it as the only source of truth.

## Practice

1. Save a theme value to `localStorage`.
2. Read the theme and apply a CSS class.
3. Store a settings object as JSON.
4. Write a safe JSON parse helper with a fallback value.

## Related Topics

- [JSON](../03_data_structures/json.md)
- [Strings](../03_data_structures/strings.md)
- [Browser APIs](browser_apis.md)

