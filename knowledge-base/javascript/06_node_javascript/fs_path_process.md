# File System, Path, and Process

## Goal

Understand common Node.js built-in utilities for files, paths, and process
information.

## Why It Matters

Node.js scripts often read configuration files, write generated output, build
safe file paths, inspect command-line arguments, and use process information.

## File System

Use `node:fs/promises` for promise-based file operations.

```javascript
import { readFile } from "node:fs/promises";

const text = await readFile("message.txt", "utf8");

console.log(text);
```

Write a file:

```javascript
import { writeFile } from "node:fs/promises";

await writeFile("output.txt", "Hello from Node.js", "utf8");
```

Read JSON:

```javascript
import { readFile } from "node:fs/promises";

const text = await readFile("user.json", "utf8");
const user = JSON.parse(text);

console.log(user.name);
```

## Path

Use `node:path` to build paths safely across operating systems.

```javascript
import path from "node:path";

const filePath = path.join("data", "users.json");

console.log(filePath);
```

Resolve an absolute path:

```javascript
const absolutePath = path.resolve("data", "users.json");

console.log(absolutePath);
```

Get file parts:

```javascript
const filePath = "data/users.json";

console.log(path.basename(filePath)); // users.json
console.log(path.extname(filePath)); // .json
console.log(path.dirname(filePath)); // data
```

## Process

`process` provides information about the current Node.js process.

```javascript
console.log(process.cwd());
console.log(process.version);
```

Command-line arguments:

```javascript
const [, , name = "Guest"] = process.argv;

console.log(`Hello, ${name}`);
```

Run:

```bash
node greet.js Alex
```

Exit codes:

```javascript
if (!process.env.API_URL) {
  console.error("API_URL is required");
  process.exit(1);
}
```

## Creating Directories

```javascript
import { mkdir, writeFile } from "node:fs/promises";
import path from "node:path";

const outputDir = path.join("dist", "reports");

await mkdir(outputDir, { recursive: true });
await writeFile(path.join(outputDir, "summary.txt"), "Done", "utf8");
```

## Real Pain Points

- Build paths with `path.join` or `path.resolve` instead of hardcoded separators.
- File operations are async when using `fs/promises`; remember to `await` them.
- `process.cwd()` depends on where the command is run from, not necessarily
  where the current file is located.
- Do not use `process.exit()` for normal control flow. Prefer returning or
  throwing unless the script truly must stop with a specific exit code.

## Practice

1. Read a text file with `readFile`.
2. Write a text file with `writeFile`.
3. Build a nested path with `path.join`.
4. Read one command-line argument from `process.argv`.

## Related Topics

- [Runtime](runtime.md)
- [Environment Variables](environment_variables.md)
- [Async and Await](../04_async_javascript/async_await.md)

