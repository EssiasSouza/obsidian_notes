Source: #source/internet_resources 
Project: #project/full_stack
Areas: #area/work
Subject: #subject/run_environments
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/learning 
Related: [[Java Script]]
[[Node.js]]

---
# # Managing Environment Variables in Modern Node.js

Every application needs configuration. Database credentials, API keys, ports, feature flags, and external service URLs should never be hardcoded into your source code.

Instead, Node.js applications use **environment variables**, allowing the same application to run in different environments without changing the code.

## What is a `.env` File?

A `.env` file is a plain text file that stores environment variables as key/value pairs.

```env
PORT=3000
DB_HOST=localhost
DB_USER=myuser
DB_PASSWORD=my-password
API_KEY=your-api-key
```

These variables become available to your application through the `process.env` object.

```javascript
console.log(process.env.PORT);
console.log(process.env.DB_HOST);
```

One important thing to remember is that your application always reads configuration from `process.env`. It doesn't matter whether the variables come from a `.env` file, Docker, Kubernetes, or a cloud secret manager.

## Loading a `.env` File

Starting with **Node.js 20.6**, Node.js includes built-in support for loading environment files. There's no need to install external packages for most projects.

Simply start your application with the `--env-file` option:

```bash
node --env-file=.env app.js
```

If you're using npm scripts:

```json
{
  "scripts": {
    "dev": "node --env-file=.env app.js"
  }
}
```

This loads every variable from the `.env` file before your application starts.

## Working with Multiple Environment Files

Most applications require different configurations depending on the environment.

A common project structure looks like this:

```text
.env
.env.local
.env.development
.env.test
.env.production
```

For example:

**.env**

```env
PORT=3000
LOG_LEVEL=info
```

**.env.local**

```env
DB_HOST=localhost
DB_PASSWORD=my-password
```

Node.js allows multiple environment files to be loaded:

```bash
node \
  --env-file=.env \
  --env-file=.env.local \
  app.js
```

This approach lets you separate shared configuration from machine-specific settings.

## Recommended File Organization

A common strategy is:

| File               | Purpose                                  |
| ------------------ | ---------------------------------------- |
| `.env`             | Default configuration shared by everyone |
| `.env.local`       | Local overrides (never committed)        |
| `.env.development` | Development environment                  |
| `.env.test`        | Test environment                         |
| `.env.production`  | Production defaults                      |
| `.env.example`     | Template containing required variables   |

The `.env.example` file should contain only variable names and placeholder values:

```env
PORT=
DB_HOST=
DB_USER=
DB_PASSWORD=
API_KEY=
```

This helps new developers understand which variables are required without exposing secrets.

## Never Commit Secrets

Your `.env` file usually contains sensitive information.

Add it to your `.gitignore` file:

```gitignore
.env
.env.local
```

Only commit the `.env.example` file.

## Do You Still Need `dotenv`?

In many modern Node.js applications, the answer is **no**.

The native `--env-file` option is enough for most use cases.

However, the `dotenv` ecosystem is still useful if you need features such as:

* Variable expansion
* Overriding existing environment variables
* Compatibility with older Node.js versions
* Additional plugins from the `dotenv` ecosystem

If your project runs on Node.js 22 or newer and only needs to load environment variables, the built-in solution is typically the simplest choice.

## What Happens in Production?

Production environments rarely use a local `.env` file.

Instead, environment variables are injected by the platform itself.

Examples include:

* Docker
* Kubernetes ConfigMaps
* Kubernetes Secrets
* External Secrets Operator (ESO)
* AWS Secrets Manager
* Google Secret Manager
* Azure Key Vault

Regardless of where the configuration comes from, your application accesses it exactly the same way:

```javascript
const apiKey = process.env.API_KEY;
```

This abstraction allows your application to move from local development to cloud environments without changing the code.

## Best Practices

* Never hardcode secrets.
* Keep configuration outside your source code.
* Commit only a `.env.example` file.
* Use the native `--env-file` option whenever possible.
* Validate required environment variables when the application starts.
* Use dedicated secret management solutions in production.

## Conclusion

Modern Node.js has made configuration management much simpler by adding native support for `.env` files. Instead of relying on external libraries, most applications can now load environment variables directly with the `--env-file` option while continuing to access them through `process.env`.

As your application grows, the source of those variables may change from a local `.env` file to Docker, Kubernetes, or a cloud secret manager, but your application code remains exactly the same.