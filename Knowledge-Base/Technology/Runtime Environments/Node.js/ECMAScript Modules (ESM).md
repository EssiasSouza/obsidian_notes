Source: #source/DIO
Project: #project/full_stack
Areas: #area/work
Subject: #subject/run_environments 
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/learning 
Related: [[Node.js]]
[[TypeScript]]

---

# Configuring a Node.js Project to Use ECMAScript Modules (ESM)

[doc](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Modules)

By default, Node.js uses the CommonJS module system (`require` and `module.exports`). To use ECMAScript Modules (ESM) with `import` and `export`, follow these steps.

## 1. Create a `package.json` File

If your project does not have a `package.json`, create one:

```bash
npm init -y
```

## 2. Enable ESM

Add `"type": "module"` to your `package.json`:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "type": "module"
}
```

This tells Node.js to treat `.js` files as ECMAScript Modules.

## 3. Create an Export

**math.js**

```javascript
export function sum(a, b) {
    return a + b;
}
```

## 4. Import the Function

**app.js**

```javascript
import { sum } from './math.js';

console.log(sum(2, 3));
```

## 5. Run the Application

```bash
node app.js
```

Output:

```text
5
```

## Default Export Example

**math.js**

```javascript
export default function sum(a, b) {
    return a + b;
}
```

**app.js**

```javascript
import sum from './math.js';

console.log(sum(2, 3));
```

## Notes

* Always include the file extension (`.js`) in imports.
* Use `import` and `export` instead of `require` and `module.exports`.
* Node.js 14+ supports ESM, but Node.js 18+ is recommended.

### CommonJS vs ESM

| CommonJS                         | ECMAScript Modules             |
| -------------------------------- | ------------------------------ |
| `const math = require('./math')` | `import math from './math.js'` |
| `module.exports = value`         | `export default value`         |
| `exports.sum = sum`              | `export function sum() {}`     |
|                                  |                                |

ESM is the modern JavaScript module standard and is recommended for new Node.js projects.
