Source: #source/DIO
Project: #project/full_stack
Areas: #area/work
Subject: #subject/run_environments
Type: #type/learning 
Learning priority: #priority/P1 
Status: #status/learning 
Related: [[Java Script]]
[[TypeScript]]

---
# Node.js

[https://nodejs.org/api/modules.html](https://nodejs.org/api/modules.html)
Package managers
[npm js]([npm | Home](https://www.npmjs.com/))
[Yarn](https://yarnpkg.com/cli/node) Is not native
[pNPM](https://pnpm.io/installation) Is not native
[75 packages](https://firebearstudio.com/blog/node-js-command-line-apps-utilities.html)
[npm scritps documentation](https://docs.npmjs.com/cli/v10/using-npm/scripts)

Node.js is a run environment to run [[Java Script]] applications like [[Python]] do. There is possible to use Java Script to create web applications, and other applications without using a browser.

# What is Node.js?

Node.js is a JavaScript runtime environment that allows developers to execute JavaScript code outside a web browser. It was built on top of the V8 JavaScript engine, the same engine used by Google Chrome, providing high performance and the ability to create server-side applications using JavaScript.

Before Node.js, JavaScript was mainly used for adding interactivity to web pages inside browsers. With Node.js, developers can use JavaScript to build backend services, APIs, command-line tools, automation scripts, and even desktop applications.

One of the main features of Node.js is its event-driven and non-blocking architecture. This means that it can handle many simultaneous connections efficiently, making it a popular choice for real-time applications such as chat systems, streaming services, and microservices.

Node.js has a large ecosystem powered by npm (Node Package Manager), which provides thousands of reusable libraries and tools that simplify application development.

It is important to understand that Node.js is not a programming language. The programming language is JavaScript, while Node.js is the runtime environment that executes JavaScript applications and provides access to system resources such as files, networks, and operating system features.

In modern software development, Node.js is widely used for building scalable web applications, APIs, automation solutions, and cloud-native services. It allows developers to use the same language across both frontend and backend development, creating a more unified development experience.

### commands

1 - Starting a project creating a `package.json`

```bash
npm init -y # default project information
```

2 - Running scripts automatically

```bash
node --watch /path/script.js
```

#### Installing packages

Installing packages
```bash
npm install <package name>
# or
npm i <package name>
```

Installing dependences that are in the `package.json`
```bash
npm install
```

Installing devDependences that are in the `package.json`
```bash
npm install <package name> -D
```




