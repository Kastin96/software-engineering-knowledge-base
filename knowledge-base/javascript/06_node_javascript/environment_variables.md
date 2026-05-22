# Environment Variables

## Goal

Understand how Node.js reads configuration from environment variables.

## Why It Matters

Applications often need different configuration for local development, testing,
staging, and production. Environment variables keep configuration outside the
source code.

## Reading Environment Variables

Node.js exposes environment variables through `process.env`.

```javascript
const port = process.env.PORT ?? "3000";

console.log(`Server will run on port ${port}`);
```

Environment variable values are always strings or `undefined`.

```javascript
const debug = process.env.DEBUG_MODE;

console.log(typeof debug); // string or undefined
```

## Providing Defaults

```javascript
const nodeEnv = process.env.NODE_ENV ?? "development";

console.log(nodeEnv);
```

Convert values when you need another type.

```javascript
const port = Number(process.env.PORT ?? 3000);

if (Number.isNaN(port)) {
  throw new Error("PORT must be a number");
}
```

Boolean values need explicit parsing.

```javascript
const enableLogs = process.env.ENABLE_LOGS === "true";

console.log(enableLogs);
```

## Required Variables

Create a helper for required configuration.

```javascript
function readRequiredEnv(name) {
  const value = process.env[name];

  if (!value) {
    throw new Error(`${name} is required`);
  }

  return value;
}

const databaseUrl = readRequiredEnv("DATABASE_URL");

console.log(databaseUrl);
```

## Using .env Files

Many projects use `.env` files locally.

```text
PORT=3000
DATABASE_URL=postgres://localhost:5432/app
```

Node.js projects commonly load these files with a package or runtime option.
Keep secrets out of Git.

```gitignore
.env
```

Commit a safe example file instead:

```text
# .env.example
PORT=3000
DATABASE_URL=
```

## Configuration Object

Centralize environment parsing.

```javascript
function readConfig() {
  return {
    nodeEnv: process.env.NODE_ENV ?? "development",
    port: Number(process.env.PORT ?? 3000),
    databaseUrl: readRequiredEnv("DATABASE_URL"),
  };
}

const config = readConfig();

console.log(config);
```

## Real Pain Points

- Environment variables are strings. Parse numbers and booleans explicitly.
- Missing required variables should fail early during startup.
- Do not commit real secrets in `.env` files.
- Avoid reading `process.env` all over the app. A central config module is
  easier to validate and test.

## Practice

1. Read `PORT` with a default value.
2. Parse a boolean environment variable.
3. Write a `readRequiredEnv` helper.
4. Create a `.env.example` with safe placeholder values.

## Related Topics

- [File System, Path, and Process](fs_path_process.md)
- [npm](npm.md)
- [Scripts](scripts.md)

