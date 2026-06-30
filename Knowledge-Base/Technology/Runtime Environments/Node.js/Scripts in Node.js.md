Source: #source/DIO
Project: #project/full_stack
Areas: #area/work
Subject: #subject/run_environments
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/learning 
Related: [[Java Script]]
[[Node.js]]
[[TypeScript]]

---
# Getting Started with npm Scripts in Node.js

`npm scripts` are one of the simplest and most powerful features of Node.js. They allow you to define custom commands inside your project's `package.json` file, making it easy to automate common development tasks.

## What are npm Scripts?

The `scripts` section of `package.json` contains named commands that can be executed using `npm run`.

Example:

```json
{
  "scripts": {
    "start": "node index.js",
    "test": "node test.js"
  }
}
```

You can execute these commands from your terminal:

```bash
npm run start
npm run test
```

For the special commands `start`, `test`, `stop`, and `restart`, the `run` keyword is optional:

```bash
npm start
npm test
```

## Why Use npm Scripts?

Using npm scripts provides several benefits:

- Standardizes commands across your team.
    
- Eliminates the need to remember long terminal commands.
    
- Automates repetitive tasks.
    
- Works on Windows, Linux, and macOS without additional tooling.
    

## Common Examples

### Run your application

```json
{
  "scripts": {
    "start": "node app.js"
  }
}
```

### Watch for file changes

```json
{
  "scripts": {
    "dev": "nodemon app.js"
  }
}
```

Run it with:

```bash
npm run dev
```

### Lint your code

```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```

### Format your code

```json
{
  "scripts": {
    "format": "prettier --write ."
  }
}
```

## Chaining Commands

You can combine multiple commands.

Run the second command only if the first succeeds:

```json
{
  "scripts": {
    "build": "npm run lint && npm run test"
  }
}
```

Run commands regardless of success:

```json
{
  "scripts": {
    "all": "npm run lint ; npm run test"
  }
}
```

## Passing Arguments

Arguments can be forwarded using `--`:

```bash
npm run start -- --port=3000
```

Your script receives:

```bash
node index.js --port=3000
```

## Best Practices

- Keep script names short and descriptive.
    
- Use standard names like `start`, `test`, `build`, and `dev` whenever possible.
    
- Store project-specific commands in `package.json` instead of external documentation.
    
- Use scripts to automate repetitive tasks and simplify onboarding for new developers.
    

## Conclusion

`npm scripts` are a lightweight automation tool built into npm. Whether you need to start your application, run tests, lint your code, or execute deployment tasks, they provide a consistent and maintainable way to manage your project's workflow without requiring additional task runners.