# Browser APIs

## Goal

Understand common browser APIs and how to use them safely.

## Why It Matters

Browser APIs let JavaScript interact with the page, URL, clipboard, viewport,
history, device capabilities, and other browser features.

## URL and URLSearchParams

Use `URL` to parse and change URLs.

```javascript
const url = new URL("https://example.com/products?page=2");

console.log(url.hostname); // example.com
console.log(url.searchParams.get("page")); // 2
```

Build query strings with `URLSearchParams`.

```javascript
const params = new URLSearchParams({
  search: "javascript",
  page: "1",
});

console.log(params.toString()); // search=javascript&page=1
```

## Location

`window.location` describes the current page URL.

```javascript
console.log(window.location.href);
console.log(window.location.pathname);
```

Navigate to another page:

```javascript
window.location.href = "/login";
```

## History

The History API can update the URL without a full page reload.

```javascript
window.history.pushState({ page: "settings" }, "", "/settings");
```

Listen for back and forward navigation:

```javascript
window.addEventListener("popstate", (event) => {
  console.log("History state:", event.state);
});
```

## Clipboard

The Clipboard API can copy text. It usually requires a secure context and user
interaction.

```javascript
async function copyText(text) {
  await navigator.clipboard.writeText(text);
}

document.querySelector("#copy").addEventListener("click", async () => {
  await copyText("Hello");
});
```

## Dialogs

Simple browser dialogs:

```javascript
alert("Saved");

const confirmed = confirm("Delete item?");

if (confirmed) {
  console.log("Item deleted");
}
```

These are useful for learning, but custom UI is usually better in real apps.

## Window Size

```javascript
console.log(window.innerWidth);
console.log(window.innerHeight);
```

Listen for resize:

```javascript
window.addEventListener("resize", () => {
  console.log(window.innerWidth);
});
```

## requestAnimationFrame

Use `requestAnimationFrame` for visual updates.

```javascript
function animate() {
  console.log("frame");
  requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```

## Feature Checks

Check whether an API exists before using it.

```javascript
if ("clipboard" in navigator) {
  console.log("Clipboard API is available");
}
```

## Real Pain Points

- Some APIs require HTTPS, user permission, or a user gesture like a click.
- Not every browser supports every API, so feature checks matter for production
  code.
- Browser dialogs block interaction. Use them carefully.
- Resize and scroll events can fire often, so expensive work should be
  debounced, throttled, or moved to `requestAnimationFrame`.

## Practice

1. Read the current page path with `window.location`.
2. Build a query string with `URLSearchParams`.
3. Copy text to the clipboard after a button click.
4. Add a feature check before using a browser API.

## Related Topics

- [Events](events.md)
- [Timers](../04_async_javascript/timers.md)
- [Fetch in the Browser](fetch_in_browser.md)
