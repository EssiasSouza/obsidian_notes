Source: #source/internet_resources 
Project: #project/web
Areas: #area/work
Subject: #subect/application
Type: #type/learning 
Learning priority: #priority/P3 
Status: #status/learning
Related: [[Vercel]]

---
# Framework ASTRO

Astro is a modern framework for building websites and web applications focused on performance.

The **`astro build`** command is responsible for generating the final production version of the project.

In practice, when you run:

```bash
astro build
```

Astro:

* reads all project files
* processes pages, components, and assets
* generates optimized HTML, CSS, and JavaScript
* performs minification and optimizations
* creates the final deployment folder (usually `dist/`)

This `dist/` folder is what you publish to servers, CDNs, or platforms such as:

* Vercel
* Netlify
* Cloudflare
* AWS

## Common Workflow

### Development

```bash
astro dev
```

* starts a local server
* enables hot reload
* provides a development environment

---

### Production Build

```bash
astro build
```

* generates the optimized version
* produces static files or SSR output depending on the configuration

---

### Preview the Build

```bash
astro preview
```

* locally simulates the production build

---

## What Is Generated

Example:

```plaintext
project/
├── src/
├── public/
├── astro.config.mjs
└── dist/   <-- created by astro build
```

---

## Build Modes

Astro supports:

### Static Site Generation (SSG)

Generates static HTML.

Ideal for:

* blogs
* documentation
* landing pages

---

### Server-Side Rendering (SSR)

Renders on the server.

Ideal for:

* authentication
* dynamic content
* dashboards

---

## Example Output

During the build, you may see something like:

```bash
✓ Completed in 2.34s

 generating static routes
▶ src/pages/index.astro
▶ src/pages/about.astro

dist/index.html
dist/about/index.html
```

---

## Configuration

The build behavior depends on the:

```bash
astro.config.mjs
```

file.

Example:

```js
import { defineConfig } from 'astro/config';

export default defineConfig({
  output: 'static'
});
```

or:

```js
export default defineConfig({
  output: 'server'
});
```

---

## In DevOps Pipelines

Since you work extensively with cloud and automation, `astro build` is commonly used in CI/CD pipelines:

```bash
npm install
npm run build
```

where the `package.json` usually contains:

```json
{
  "scripts": {
    "build": "astro build"
  }
}
```

After that, the pipeline may:

* publish the `dist/`
* upload files to a bucket/CDN
* build a container image
* deploy to Kubernetes
* etc.

