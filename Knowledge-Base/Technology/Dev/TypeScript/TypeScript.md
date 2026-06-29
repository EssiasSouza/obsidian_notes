Source: #source/internet_resources 
Project: #project/devops 
Areas: #area/work
Subject: #subject/java 
Type: #type/learning
Learning priority: #priority/P2 
Status: #status/learning 
Related: [[Java Script]]

---

# TypeScript

https://typescriptlang.org/

https://typescriptlang.org/cheatsheets

TypeScritpt is a superset that is used over JavaScript
### Transpile 

Translate and compile

### Installing

```bash
npm install -D typescript
```

The files need to be with extension .ts.

Example:

```bash
script.ts
```


Do transpile

```bash
npx tsc path/to/script.ts
```

To `transpile` automatically maybe used the `package.json` scripts to call

```json
...{
	"scripts": {
		"dist": "npx tsc src/index.ts",
		"start:dev": npm run dist && node src/index.js
	}
}
```

#### Tsconfig

Creating `tsconfig.json` in the project root folder.

```bash
npx tsc --init
```

Configuring tsconfig.json

```json

{
	"compilerOpitions":{
		"target": "ES6", //Ecma script 6
		"module": "CommonJS",
		"outDir": "./dist", //to save the transpiled files.
		"strict": true,
		"esModuleInterop": true,
	}
}
```

# Became node.js to run ts

Install tsx

```bash
npm i tsx -D # - D to rund as dependencies but only for devel environment.
```

