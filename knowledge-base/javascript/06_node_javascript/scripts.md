# Scripts

## Goal

Understand how npm scripts automate common project tasks.

## Why It Matters

npm scripts are used to start apps, run tests, format code, build projects,
generate files, run migrations, and simplify repeated commands.

## Basic Scripts

Scripts live in `package.json`.

```json
{
  "scripts": {
    "start": "node src/app.js",
    "dev": "node --watch src/app.js",
    "test": "node --test"
  }
}
```

Run a script:

```bash
npm run dev
```

Some common scripts have shortcuts:

```bash
npm start
npm test
```

## Passing Arguments

Use `--` to pass arguments to the script command.

```json
{
  "scripts": {
    "greet": "node scripts/greet.js"
  }
}
```

Run:

```bash
npm run greet -- Alex
```

Read the argument:

```javascript
const [, , name = "Guest"] = process.argv;

console.log(`Hello, ${name}`);
```

## Using Local Binaries

Packages installed in `node_modules` can expose command-line tools. npm scripts
can run them without a global install.

```json
{
  "scripts": {
    "test": "vitest",
    "format": "prettier . --write"
  }
}
```

## Script Naming

Use clear names for common workflows.

```json
{
  "scripts": {
    "dev": "node --watch src/app.js",
    "start": "node src/app.js",
    "test": "node --test",
    "lint": "eslint .",
    "format": "prettier . --write",
    "build": "node scripts/build.js"
  }
}
```

## Environment Variables in Scripts

Cross-platform environment variables can be tricky because shells differ.
For simple Node scripts, prefer reading config from the environment and document
how to set it for your project.

```javascript
const nodeEnv = process.env.NODE_ENV ?? "development";

console.log(nodeEnv);
```

## Pre and Post Scripts

npm can run scripts before or after another script when named with `pre` or
`post`.

```json
{
  "scripts": {
    "prebuild": "node scripts/clean.js",
    "build": "node scripts/build.js",
    "postbuild": "node scripts/summary.js"
  }
}
```

Running `npm run build` also runs `prebuild` before and `postbuild` after.

## Real Pain Points

- Scripts are part of the project interface. Keep names predictable and
  documented.
- Avoid depending on globally installed tools when a project dependency is more
  reproducible.
- Shell syntax differs between Windows, macOS, and Linux. Keep scripts portable
  when the project is used across operating systems.
- Very long scripts become hard to maintain. Move complex logic into a
  JavaScript file.

## Practice

1. Add a `start` script that runs a Node file.
2. Add a `dev` script with `node --watch`.
3. Add a script that accepts a command-line argument.
4. Move a long command into a script file and call it from `package.json`.

## Related Topics

- [npm](npm.md)
- [Environment Variables](environment_variables.md)
- [File System, Path, and Process](fs_path_process.md)
