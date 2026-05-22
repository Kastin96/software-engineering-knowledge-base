# npm

## Goal

Understand what npm is and how Node.js projects use `package.json`.

## Why It Matters

Most JavaScript projects use npm or a compatible package manager to install
dependencies, run scripts, and describe project metadata.

## Explanation

npm is a package manager for JavaScript. It helps install third-party packages
and manage project scripts.

Create a new `package.json`:

```bash
npm init -y
```

Example `package.json`:

```json
{
  "name": "example-project",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "start": "node src/app.js"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

## Installing Dependencies

Install a runtime dependency:

```bash
npm install express
```

This adds the package to `dependencies`.

Install a development dependency:

```bash
npm install --save-dev vitest
```

This adds the package to `devDependencies`.

## dependencies vs devDependencies

`dependencies` are needed by the application at runtime.

```json
{
  "dependencies": {
    "express": "^4.18.0"
  }
}
```

`devDependencies` are needed for development tasks such as testing, formatting,
or building.

```json
{
  "devDependencies": {
    "vitest": "^1.0.0"
  }
}
```

## package-lock.json

`package-lock.json` records exact installed dependency versions. Commit it for
applications so installs are more reproducible.

```bash
npm install
```

This reads `package.json` and `package-lock.json` and installs dependencies into
`node_modules`.

## Running Package Scripts

```json
{
  "scripts": {
    "start": "node src/app.js",
    "test": "vitest"
  }
}
```

Run scripts:

```bash
npm run start
npm test
```

## Real Pain Points

- Do not commit `node_modules`; commit `package.json` and usually
  `package-lock.json`.
- A project can break when dependency versions drift. Lockfiles help reduce
  that risk.
- Install tools used only for development as `devDependencies`, not
  `dependencies`.

## Practice

1. Create a `package.json` with `npm init -y`.
2. Add a `start` script.
3. Install one runtime dependency.
4. Install one development dependency.

## Related Topics

- [Scripts](scripts.md)
- [Runtime](runtime.md)
- [CommonJS vs ES Modules](commonjs_vs_esm.md)

