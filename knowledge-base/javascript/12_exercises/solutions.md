# Solutions

These are example solutions. They are not the only correct answers.

## Beginner

### 1. Normalize Email

```javascript
function normalizeEmail(email) {
  return email.trim().toLowerCase();
}
```

### 2. Calculate Cart Total

```javascript
function calculateCartTotal(items) {
  return items.reduce((total, item) => {
    return total + item.price * item.quantity;
  }, 0);
}
```

### 3. Get Active Users

```javascript
function getActiveUsers(users) {
  return users.filter((user) => user.active);
}
```

### 4. Format User Label

```javascript
function formatUserLabel(user) {
  return `${user.firstName} ${user.lastName} <${user.email}>`;
}
```

### 5. Count Words

```javascript
function countWords(text) {
  const words = text.trim().split(/\s+/);

  if (words.length === 1 && words[0] === "") {
    return 0;
  }

  return words.length;
}
```

### 6. Create Slug

```javascript
function createSlug(title) {
  return title
    .trim()
    .toLowerCase()
    .replace(/\s+/g, "-")
    .replace(/^-+|-+$/g, "");
}
```

### 7. Find User by Id

```javascript
function findUserById(users, id) {
  return users.find((user) => user.id === id) ?? null;
}
```

### 8. Build Initials

```javascript
function getInitials(fullName) {
  return fullName
    .trim()
    .split(/\s+/)
    .map((part) => part[0].toUpperCase())
    .join("");
}
```

### 9. Safe Number Parse

```javascript
function parsePrice(value) {
  const price = Number(value);
  return Number.isNaN(price) ? 0 : price;
}
```

### 10. Has Permission

```javascript
function hasPermission(user, permission) {
  return user.permissions.includes(permission);
}
```

## Intermediate

### 1. Group Users by Role

```javascript
function groupUsersByRole(users) {
  return users.reduce((groups, user) => {
    groups[user.role] ??= [];
    groups[user.role].push(user);
    return groups;
  }, {});
}
```

### 2. Normalize API Users

```javascript
function normalizeApiUsers(apiUsers) {
  return apiUsers.map((user) => ({
    id: String(user.user_id),
    fullName: `${user.first_name} ${user.last_name}`,
    email: user.email_address.trim().toLowerCase(),
    active: Boolean(user.is_active),
  }));
}
```

### 3. Validate Product

```javascript
function validateProduct(input) {
  const errors = {};

  if (!input.name.trim()) {
    errors.name = "Name is required";
  }

  if (typeof input.price !== "number" || input.price <= 0) {
    errors.price = "Price must be greater than 0";
  }

  if (!/^[A-Z0-9-]+$/.test(input.sku)) {
    errors.sku = "SKU format is invalid";
  }

  return errors;
}
```

### 4. Update User in List

```javascript
function updateUserById(users, id, updates) {
  return users.map((user) => {
    if (user.id !== id) {
      return user;
    }

    return {
      ...user,
      ...updates,
    };
  });
}
```

### 5. Build Query String

```javascript
function buildQueryString(params) {
  const searchParams = new URLSearchParams();

  for (const [key, value] of Object.entries(params)) {
    if (value === null || value === undefined || value === "") {
      continue;
    }

    searchParams.set(key, String(value));
  }

  return searchParams.toString();
}
```

### 6. Create Pagination Metadata

```javascript
function createPaginationMeta({ page, pageSize, totalItems }) {
  const safePageSize = Math.max(1, pageSize);
  const totalPages = Math.max(1, Math.ceil(totalItems / safePageSize));
  const currentPage = Math.min(Math.max(1, page), totalPages);

  return {
    page: currentPage,
    pageSize: safePageSize,
    totalItems,
    totalPages,
    hasPreviousPage: currentPage > 1,
    hasNextPage: currentPage < totalPages,
  };
}
```

### 7. Retry Async Operation

```javascript
async function retry(operation, attempts) {
  let lastError;

  for (let attempt = 1; attempt <= attempts; attempt += 1) {
    try {
      return await operation();
    } catch (error) {
      lastError = error;
    }
  }

  throw lastError;
}
```

### 8. Convert List to Lookup

```javascript
function createLookup(items, key) {
  return items.reduce((lookup, item) => {
    lookup[item[key]] = item;
    return lookup;
  }, {});
}
```

## Advanced

### 1. Search Result State Manager

```javascript
const initialSearchState = {
  query: "",
  status: "idle",
  results: [],
  error: null,
};

function setQuery(state, query) {
  return {
    ...state,
    query,
  };
}

function startLoading(state) {
  return {
    ...state,
    status: "loading",
    error: null,
  };
}

function setSuccess(state, results) {
  return {
    ...state,
    status: "success",
    results,
    error: null,
  };
}

function setError(state, error) {
  return {
    ...state,
    status: "error",
    error,
  };
}
```

### 2. Stale Request Guard

```javascript
function createLatestOnlyRunner() {
  let latestId = 0;

  return async function runLatest(operation) {
    const id = latestId + 1;
    latestId = id;

    const result = await operation();

    if (id !== latestId) {
      return null;
    }

    return result;
  };
}
```

### 3. Config Reader

```javascript
function readRequiredEnv(env, name) {
  const value = env[name];

  if (!value) {
    throw new Error(`${name} is required`);
  }

  return value;
}

function createConfig(env) {
  const port = Number(env.PORT ?? 3000);

  if (Number.isNaN(port)) {
    throw new Error("PORT must be a number");
  }

  return {
    databaseUrl: readRequiredEnv(env, "DATABASE_URL"),
    port,
    enableLogs: env.ENABLE_LOGS === "true",
  };
}
```

### 4. Event Emitter

```javascript
function createEventEmitter() {
  const listenersByEvent = new Map();

  function on(eventName, listener) {
    const listeners = listenersByEvent.get(eventName) ?? new Set();
    listeners.add(listener);
    listenersByEvent.set(eventName, listeners);

    return () => off(eventName, listener);
  }

  function off(eventName, listener) {
    listenersByEvent.get(eventName)?.delete(listener);
  }

  function emit(eventName, payload) {
    const listeners = listenersByEvent.get(eventName) ?? [];

    for (const listener of listeners) {
      try {
        listener(payload);
      } catch (error) {
        console.error(`Listener failed for ${eventName}`, error);
      }
    }
  }

  return {
    on,
    off,
    emit,
  };
}
```

### 5. In-Memory Repository

```javascript
function cloneUser(user) {
  return structuredClone(user);
}

function createUserRepository() {
  const users = new Map();
  let nextId = 1;

  return {
    create(user) {
      const createdUser = {
        ...user,
        id: String(nextId),
      };

      nextId += 1;
      users.set(createdUser.id, cloneUser(createdUser));

      return cloneUser(createdUser);
    },
    findById(id) {
      const user = users.get(id);
      return user ? cloneUser(user) : null;
    },
    update(id, updates) {
      const user = users.get(id);

      if (!user) {
        return null;
      }

      const updatedUser = {
        ...user,
        ...updates,
        id,
      };

      users.set(id, cloneUser(updatedUser));
      return cloneUser(updatedUser);
    },
    delete(id) {
      return users.delete(id);
    },
  };
}
```

### 6. Request Queue With Concurrency Limit

```javascript
async function runWithConcurrencyLimit(tasks, limit) {
  const results = new Array(tasks.length);
  let nextIndex = 0;

  async function worker() {
    while (nextIndex < tasks.length) {
      const currentIndex = nextIndex;
      nextIndex += 1;

      results[currentIndex] = await tasks[currentIndex]();
    }
  }

  const workers = Array.from(
    { length: Math.min(limit, tasks.length) },
    () => worker(),
  );

  await Promise.all(workers);

  return results;
}
```

### 7. Form Validation Engine

```javascript
function required(message) {
  return (value) => {
    if (value === null || value === undefined || String(value).trim() === "") {
      return message;
    }

    return null;
  };
}

function email(message) {
  return (value) => {
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(String(value))) {
      return message;
    }

    return null;
  };
}

function minLength(length, message) {
  return (value) => {
    if (String(value).length < length) {
      return message;
    }

    return null;
  };
}

function validateFields(values, rules) {
  const errors = {};

  for (const [field, fieldRules] of Object.entries(rules)) {
    const fieldErrors = fieldRules
      .map((rule) => rule(values[field]))
      .filter(Boolean);

    if (fieldErrors.length > 0) {
      errors[field] = fieldErrors;
    }
  }

  return errors;
}
```

### 8. Normalized Store

```javascript
function addOne(state, item) {
  if (state.entities[item.id]) {
    return updateOne(state, item.id, item);
  }

  return {
    ids: [...state.ids, item.id],
    entities: {
      ...state.entities,
      [item.id]: item,
    },
  };
}

function updateOne(state, id, updates) {
  const existing = state.entities[id];

  if (!existing) {
    return state;
  }

  return {
    ...state,
    entities: {
      ...state.entities,
      [id]: {
        ...existing,
        ...updates,
        id,
      },
    },
  };
}

function removeOne(state, id) {
  if (!state.entities[id]) {
    return state;
  }

  const { [id]: removed, ...entities } = state.entities;

  return {
    ids: state.ids.filter((itemId) => itemId !== id),
    entities,
  };
}
```
