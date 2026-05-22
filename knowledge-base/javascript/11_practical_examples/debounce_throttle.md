# Debounce and Throttle

## Goal

Control how often a function runs during frequent events such as typing,
scrolling, resizing, and mouse movement.

## Debounce

Debounce waits until calls stop for a delay.

Use it for search input, autosave, and validation after typing.

```javascript
function debounce(callback, delay) {
  let timeoutId;

  return function debounced(...args) {
    clearTimeout(timeoutId);

    timeoutId = setTimeout(() => {
      callback.apply(this, args);
    }, delay);
  };
}
```

Usage:

```javascript
const searchInput = document.querySelector("#search");

const handleSearch = debounce(async (event) => {
  const query = event.target.value.trim();

  if (!query) {
    renderResults([]);
    return;
  }

  const results = await searchProducts(query);
  renderResults(results);
}, 300);

searchInput.addEventListener("input", handleSearch);
```

## Debounce With Cancellation

```javascript
function debounceWithCancel(callback, delay) {
  let timeoutId;

  function debounced(...args) {
    clearTimeout(timeoutId);

    timeoutId = setTimeout(() => {
      callback.apply(this, args);
    }, delay);
  }

  debounced.cancel = () => {
    clearTimeout(timeoutId);
  };

  return debounced;
}
```

Usage:

```javascript
const saveDraft = debounceWithCancel((draft) => {
  localStorage.setItem("draft", JSON.stringify(draft));
}, 500);

saveDraft({ title: "JavaScript notes" });
saveDraft.cancel();
```

## Throttle

Throttle runs at most once during a time window.

Use it for scroll, resize, and pointer movement.

```javascript
function throttle(callback, delay) {
  let lastRun = 0;

  return function throttled(...args) {
    const now = Date.now();

    if (now - lastRun < delay) {
      return;
    }

    lastRun = now;
    callback.apply(this, args);
  };
}
```

Usage:

```javascript
const handleScroll = throttle(() => {
  console.log("scroll position", window.scrollY);
}, 200);

window.addEventListener("scroll", handleScroll);
```

## Throttle With Trailing Call

This version runs immediately, then remembers the latest call and runs once more
at the end of the delay.

```javascript
function throttleWithTrailing(callback, delay) {
  let lastRun = 0;
  let timeoutId;
  let latestArgs;
  let latestThis;

  return function throttled(...args) {
    const now = Date.now();
    const remaining = delay - (now - lastRun);

    latestArgs = args;
    latestThis = this;

    if (remaining <= 0) {
      clearTimeout(timeoutId);
      timeoutId = undefined;
      lastRun = now;
      callback.apply(latestThis, latestArgs);
      return;
    }

    if (!timeoutId) {
      timeoutId = setTimeout(() => {
        lastRun = Date.now();
        timeoutId = undefined;
        callback.apply(latestThis, latestArgs);
      }, remaining);
    }
  };
}
```

## Choosing Between Them

Use debounce when only the final value matters.

```javascript
const debouncedValidate = debounce(validateForm, 300);
```

Use throttle when ongoing updates matter but should be limited.

```javascript
const throttledResize = throttle(updateLayout, 200);
```

## What This Demonstrates

- Closures for keeping timer state.
- `setTimeout` and `clearTimeout`.
- Preserving `this` and arguments with `apply`.
- Choosing behavior based on user interaction.

## Practice

1. Debounce a search input.
2. Throttle a scroll handler.
3. Add a `cancel` method to a debounce helper.
4. Write tests with fake timers for debounce behavior.

## Related Topics

- [Timers](../04_async_javascript/timers.md)
- [Closures](../02_core_concepts/closures.md)
- [Events](../05_browser_javascript/events.md)
