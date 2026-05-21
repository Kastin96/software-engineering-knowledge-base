# HTML, CSS, and Browser Basics Interview Questions

## What this document covers

This document covers practical HTML, CSS, and browser basics for full-stack interviews. It focuses on page structure, styling, layout, forms, the DOM, browser storage, DevTools, and common frontend debugging steps.

## Interview Questions

1. **What is HTML?**

   HTML stands for HyperText Markup Language. It defines the structure and content of a web page.

   ```html
   <h1>User Profile</h1>
   <p>Welcome to the application.</p>
   ```

2. **What is CSS?**

   CSS stands for Cascading Style Sheets. It controls how HTML elements look, including colors, spacing, layout, and fonts.

   ```css
   h1 {
     color: blue;
   }
   ```

3. **What is semantic HTML?**

   Semantic HTML uses elements that describe their meaning, such as `header`, `main`, `section`, `article`, `nav`, and `footer`. It improves readability, accessibility, and SEO.

   ```html
   <main>
     <section>
       <h1>Orders</h1>
     </section>
   </main>
   ```

4. **What is the difference between `div` and `span`?**

   `div` is a block-level container. `span` is an inline container. Use `div` for larger layout sections and `span` for small inline text styling.

   ```html
   <div>
     User status: <span class="status">Active</span>
   </div>
   ```

5. **What are `form`, `input`, and `button` used for?**

   `form` groups user input fields. `input` collects data. `button` triggers an action, such as submitting the form.

   ```html
   <form>
     <label for="email">Email</label>
     <input id="email" name="email" type="email" />

     <button type="submit">Save</button>
   </form>
   ```

6. **What is the box model?**

   The box model describes how every element is sized. It includes content, padding, border, and margin.

   ```css
   .card {
     margin: 16px;
     border: 1px solid #cccccc;
     padding: 16px;
     width: 300px;
   }
   ```

7. **What is the difference between margin and padding?**

   Margin is space outside an element. Padding is space inside an element, between the content and the border.

   ```css
   .box {
     margin: 20px;
     padding: 12px;
   }
   ```

8. **What is the difference between block and inline elements?**

   Block elements usually start on a new line and take the full available width. Inline elements stay within the current line and take only the space they need.

   ```html
   <div>This is block</div>
   <span>This is inline</span>
   ```

9. **What is the difference between class and id?**

   A class can be reused on many elements. An id should be unique on the page. In CSS, classes use `.className`, and ids use `#idName`.

   ```html
   <p class="message">Saved successfully</p>
   <section id="profile">Profile content</section>
   ```

   ```css
   .message {
     color: green;
   }

   #profile {
     border: 1px solid #cccccc;
   }
   ```

10. **What is Flexbox?**

    Flexbox is a CSS layout system for arranging items in a row or column. It is useful for alignment, spacing, and responsive layouts.

    ```html
    <div class="toolbar">
      <button>Cancel</button>
      <button>Save</button>
    </div>
    ```

    ```css
    .toolbar {
      display: flex;
      justify-content: flex-end;
      align-items: center;
      gap: 8px;
    }
    ```

11. **What is responsive design?**

    Responsive design means the page adapts to different screen sizes, such as desktop, tablet, and mobile.

12. **What are media queries?**

    Media queries apply CSS only when certain screen conditions are met, such as a maximum width.

    ```css
    .layout {
      display: flex;
      gap: 16px;
    }

    @media (max-width: 600px) {
      .layout {
        flex-direction: column;
      }
    }
    ```

13. **What is the DOM?**

    The DOM stands for Document Object Model. It is the browser's object representation of the HTML page, which JavaScript can read and change.

14. **How does JavaScript interact with the DOM?**

    JavaScript can find elements, read values, change text, update styles, and attach event handlers.

    ```html
    <p id="message">Hello</p>
    ```

    ```javascript
    const message = document.querySelector("#message");
    message.textContent = "Updated message";
    ```

15. **What is browser DevTools?**

    Browser DevTools are built-in tools for inspecting HTML, CSS, JavaScript, network requests, console logs, performance, and storage.

16. **What is the Network tab used for?**

    The Network tab is used to inspect HTTP requests and responses. It helps debug API calls, status codes, headers, payloads, and response bodies.

17. **What is `localStorage`?**

    `localStorage` stores key-value data in the browser with no automatic expiration. Data remains after closing the browser.

    ```javascript
    localStorage.setItem("theme", "dark");
    const theme = localStorage.getItem("theme");
    ```

18. **What is `sessionStorage`?**

    `sessionStorage` stores key-value data for one browser tab session. Data is cleared when the tab is closed.

    ```javascript
    sessionStorage.setItem("draft", "Hello");
    const draft = sessionStorage.getItem("draft");
    ```

19. **What are cookies?**

    Cookies are small pieces of data stored by the browser and often sent with HTTP requests. They are commonly used for sessions, authentication, and tracking preferences.

    ```text
    Cookie: sessionId=<session-id>
    ```

20. **What are common frontend debugging steps?**

    Common steps include checking the Console for JavaScript errors, using the Elements tab to inspect HTML and CSS, checking the Network tab for failed API calls, verifying request and response data, testing on different screen sizes, and confirming browser storage values.

## Compact Examples

### Simple Form

```html
<form>
  <label for="name">Name</label>
  <input id="name" name="name" type="text" />

  <label for="email">Email</label>
  <input id="email" name="email" type="email" />

  <button type="submit">Create user</button>
</form>
```

### Flexbox Layout

```html
<div class="cards">
  <article>User card</article>
  <article>Order card</article>
  <article>Report card</article>
</div>
```

```css
.cards {
  display: flex;
  gap: 16px;
  align-items: stretch;
}

.cards article {
  flex: 1;
  padding: 16px;
  border: 1px solid #cccccc;
}
```

### Responsive Media Query

```css
.cards {
  display: flex;
  gap: 16px;
}

@media (max-width: 700px) {
  .cards {
    flex-direction: column;
  }
}
```

### Simple DOM Query

```html
<button id="saveButton">Save</button>
<p id="statusMessage"></p>
```

```javascript
const saveButton = document.querySelector("#saveButton");
const statusMessage = document.querySelector("#statusMessage");

saveButton.addEventListener("click", () => {
  statusMessage.textContent = "Saved successfully";
});
```
