Source: #source/DIO
Project: #project/devops
Areas: #area/work
Subject: #subject/java
Type: #type/learning
Learning priority: #priority/P2
Status: #status/learning
Related: [[JavaScript]]
[[Node.js]]

---

# TypeScript

Official website

https://www.typescriptlang.org

Cheat Sheets

https://www.typescriptlang.org/cheatsheets

Playground

https://www.typescriptlang.org/play

## What is TypeScript?

TypeScript is a superset of JavaScript that adds static typing and additional language features. It is transpiled into plain JavaScript before execution.

## Transpilation

Transpilation is the process of translating TypeScript code into JavaScript.

## Installation

Install TypeScript as a development dependency.

```bash
npm install -D typescript
````

TypeScript source files use the `.ts` extension.

Example:

```text
src/index.ts
```

## Transpile a file

Compile a single TypeScript file.

```bash
npx tsc src/index.ts
```

## Automating transpilation

You can automate compilation using npm scripts.

```json
{
  "scripts": {
    "build": "tsc",
    "dev": "npm run build && node dist/index.js"
  }
}
```

## tsconfig.json

Documentation

[https://www.typescriptlang.org/tsconfig](https://www.typescriptlang.org/tsconfig)

Create the configuration file.

```bash
npx tsc --init
```

Example configuration:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true
  }
}
```

### Common compiler options

| Option          | Description                                  |
| --------------- | -------------------------------------------- |
| target          | JavaScript version to generate               |
| module          | Module system (CommonJS, ESNext, etc.)       |
| outDir          | Output directory for compiled files          |
| strict          | Enables all strict type checking             |
| esModuleInterop | Improves compatibility with CommonJS modules |

# Running TypeScript without compiling

Instead of manually compiling with `tsc`, you can use **tsx**, which executes TypeScript files directly.

Install:

```bash
npm install -D tsx
```

Run a file:

```bash
tsx src/index.ts
```

Watch mode:

```bash
tsx watch src/index.ts
```

This is the most common choice for local development.

# npm Trends

[https://npmtrends.com](https://npmtrends.com)

Useful for comparing package popularity and adoption across the npm ecosystem.

# tsup

**tsup** is a fast bundler for TypeScript projects built on top of **esbuild**. It is widely used for building libraries and CLI applications.

Install:

```bash
npm install -D tsup
```

Example build:

```bash
tsup src/index.ts --format esm,cjs --dts
```

## Creating a new TypeScript project

Initialize a Node.js project:

```bash
npm init -y
```

Install TypeScript:

```bash
npm install -D typescript
```

Generate the configuration:

```bash
npx tsc --init
```

Alternatively, you can use a starter template or a framework such as Vite, NestJS, or Next.js when appropriate.
