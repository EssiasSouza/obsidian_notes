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

|Aspect|CommonJS (CJS)|ECMAScript Modules (ESM)|
|---|---|---|
|**Syntax**|`require()` and `module.exports`|`import` and `export`|
|**Example Import**|`const fs = require('fs')`|`import fs from 'fs'`|
|**Example Export**|`module.exports = myFunction`|`export default myFunction`|
|**Loading Behavior**|Synchronous|Asynchronous (supports static analysis)|
|**Standard**|Node.js specific (originally)|Official JavaScript standard|
|**Browser Support**|Not supported natively|Supported natively in modern browsers|
|**Tree Shaking**|Not supported effectively|Supported by bundlers (Webpack, Rollup, Vite)|
|**Static Analysis**|Difficult|Easy because imports are known at parse time|
|**Top-Level Await**|Not supported|Supported|
|**Dynamic Import**|`require()`|`import()`|
|**Performance**|Fast for traditional Node.js applications|Better optimization opportunities|
|**File Extension**|Usually `.js`|`.mjs` or `.js` with `"type": "module"`|
|**JSON Import**|`require('./file.json')`|`import data from './file.json' with { type: 'json' }`|
|**__dirname / __filename**|Available by default|Not available directly|
|**Module Scope**|Wrapped in a function scope|Uses module scope|
|**Circular Dependencies**|Works but can return partial exports|Better handling through live bindings|
|**Interoperability**|Can import some ESM with limitations|Can import CJS, but behavior may vary|
|**Current Recommendation**|Legacy compatibility|Recommended for new projects|

## Side-by-Side Example

### CommonJS (CJS)

```javascript
// math.js
function sum(a, b) {
    return a + b;
}

module.exports = { sum };
```

```javascript
// app.js
const sum = require('./math');

// to choose the function (destructuring)
const { sum, division } = require('./math');

console.log(sum(2, 3));
```

#### Default export

```JavaScript
// math.js  
exports.sum = (a, b) => {  
	return a + b;  
};
```

importing...:

```JavaScript
// app.js  
const { sum } = require('./math');  
  
console.log(sum(2, 3));
```

> When using `exports.sum`, the function is a named export and must be imported with destructuring: `const { sum } = require('./math')`. When using `module.exports = ...`, the function becomes the default export and can be imported directly with `const sum = require('./math')`.
### ECMAScript Modules (ESM)

```javascript
// math.js
export function sum(a, b) {
    return a + b;
}
```

```javascript
// app.js
import { sum } from './math.js';

console.log(sum(2, 3));
```

## Node.js Configuration

### Using CommonJS

```json
{
  "name": "my-project"
}
```

or explicitly:

```json
{
  "type": "commonjs"
}
```

### Using ESM

```json
{
  "type": "module"
}
```

## When to Use Each

| Scenario                       | Recommendation |
| ------------------------------ | -------------- |
| New Node.js project            | ESM            |
| Browser applications           | ESM            |
| TypeScript projects            | ESM            |
| Legacy Node.js applications    | CJS            |
| Projects with old dependencies | CJS or hybrid  |
| Modern libraries/packages      | ESM            |

## Migration Example

**Before (CJS)**

```javascript
const express = require('express');
module.exports = app;
```

**After (ESM)**

```javascript
import express from 'express';
export default app;
```

Today, ESM is the preferred choice because it is the official JavaScript module system, works in both Node.js and browsers, enables tree-shaking, and supports modern features like top-level `await`. CommonJS remains important mainly for maintaining older Node.js applications and compatibility with legacy packages.
