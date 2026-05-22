# Node.js JavaScript

This section explains how JavaScript works in Node.js.

After finishing it, you should understand what Node.js is, how to run scripts,
how npm projects are organized, how CommonJS and ES modules differ, how to work
with files and paths, how to read environment variables, and how npm scripts are
used in real projects.

## Topics

- 01\. [Runtime](runtime.md)
- 02\. [npm](npm.md)
- 03\. [CommonJS vs ES Modules](commonjs_vs_esm.md)
- 04\. [File System, Path, and Process](fs_path_process.md)
- 05\. [Environment Variables](environment_variables.md)
- 06\. [Scripts](scripts.md)

## Suggested Learning Flow

Start with the runtime so you understand what Node.js provides. Then learn npm
because most Node projects are managed through `package.json`. After that,
study modules, file system tools, environment variables, and scripts because
they appear in almost every backend or tooling project.

## Mini Goal

By the end of this section, try to create a small Node.js script that:

- reads a JSON file;
- uses `path` to build file paths safely;
- reads one environment variable;
- exports and imports a helper function;
- runs through an npm script.

