Source: 
Project: 
Areas: #area/work
Subject: 
Type: #type/idea
Learning priority: 
Status: 
Related: 

---


# Git & GitHub Workflow Guide for a Node.js Project

This guide demonstrates a complete Git and GitHub workflow using a simple Node.js project as an example.

## Topics Covered

* Initialize a Git repository
* Connect a local repository to GitHub
* Stage and commit changes
* Push and pull changes
* Work with branches
* Merge branches
* Resolve merge conflicts
* Create GitHub Issues
* Configure GitHub Actions
* Create Pull Requests
* Enable Dependabot

---

# 1. Initialize a Local Git Repository

Initialize Git:

```bash
git init
```

Rename the default branch:

```bash
git branch -M main
```

Connect the repository to GitHub:

```bash
git remote add origin https://github.com/<username>/<repository>.git
```

Verify the configuration:

```bash
git remote -v
git status
```

Stage the project files:

```bash
git add .
```

Create the initial commit:

```bash
git commit -m "Initial project structure"
```

View the commit history:

```bash
git log
```

Push the repository:

```bash
git push -u origin main
```

---

# 2. Synchronize Remote Changes

Suppose someone updates the project documentation directly on GitHub.

Pull the latest changes:

```bash
git pull origin main
```

Check the repository status:

```bash
git status
```

---

# 3. Create a Feature Branch

List available branches:

```bash
git branch
```

Create and switch to a new branch:

```bash
git checkout -b feature/add-tests
```

Confirm the active branch:

```bash
git branch
```

---

# 4. Add Automated Tests

Example project structure:

```
project/
├── src/
├── tests/
├── package.json
└── README.md
```

Example test file:

```javascript
// tests/app.test.js

describe("Application", () => {
  test("should return true", () => {
    expect(true).toBe(true);
  });
});
```

Run the tests:

```bash
npm test
```

Stage and commit the changes:

```bash
git add .
git commit -m "Add initial test suite"
```

Review the commit history:

```bash
git log --oneline
```

Push the branch:

```bash
git push origin feature/add-tests
```

---

# 5. Merge a Feature Branch

Switch back to the main branch:

```bash
git checkout main
```

Merge the feature branch:

```bash
git merge feature/add-tests
```

Verify the history:

```bash
git log --oneline --graph
```

Push the updated main branch:

```bash
git push origin main
```

Delete the local branch:

```bash
git branch -d feature/add-tests
```

Delete the remote branch:

```bash
git push origin --delete feature/add-tests
```

---

# 6. Resolve Merge Conflicts

Imagine two developers modify the same file.

Example file:

```javascript
// src/server.js

console.log("Server started.");
```

One branch changes it to:

```javascript
console.log("Application is running.");
```

Another branch changes it to:

```javascript
console.log("Server is listening on port 3000.");
```

When pulling or merging, Git may report a conflict.

Retrieve the latest changes:

```bash
git pull origin main
```

Open the conflicted file and choose the appropriate version, or combine both changes.

After resolving the conflict:

```bash
git add .
git commit
git push origin main
```

---

# 7. Create a GitHub Issue

Example Issue:

**Title**

```
Improve API response messages
```

**Description**

```markdown
# Improvement

Update API response messages to make them more descriptive and user-friendly.
```

Recommended actions:

* Assign the Issue to a team member.
* Add labels such as:

```
enhancement
documentation
bug
```

---

# 8. Configure GitHub Actions

Create the workflow:

```
.github/workflows/ci.yml
```

Example:

```yaml
name: Continuous Integration

on:
  pull_request:
    branches:
      - main

jobs:
  test:

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 24

      - run: npm install
      - run: npm test
```

Commit the workflow:

```bash
git add .
git commit -m "Add CI workflow"
git push origin main
```

---

# 9. Open a Pull Request

Create a new branch:

```bash
git checkout -b feature/update-server
```

Modify a file:

```javascript
// src/server.js

console.log("Application is ready.");
```

Commit the changes:

```bash
git add .
git commit -m "Update startup message"
git push origin feature/update-server
```

Open a Pull Request on GitHub:

1. Select **New Pull Request**.
2. Set **main** as the base branch.
3. Select **feature/update-server** as the compare branch.
4. Review the changes.
5. Create the Pull Request.
6. Wait for the CI workflow to complete.
7. Merge the Pull Request.

---

# 10. Protect the Main Branch

Navigate to:

```
Settings
  → Branches
      → Add Branch Ruleset
```

Recommended rules:

* Require a Pull Request before merging.
* Require status checks to pass.
* Require at least one approval.
* Prevent force pushes.

---

# 11. Enable Dependabot

Navigate to:

```
Settings
  → Security
      → Advanced Security
```

Enable:

* Dependency Graph
* Dependabot Alerts
* Dependabot Security Updates

You can then review:

```
Security
  → Dependabot Alerts
```

and

```
Insights
  → Dependency Graph
```

When vulnerabilities are detected, Dependabot can automatically create Pull Requests with dependency updates.
