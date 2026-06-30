Source: #source/internet_resources
Project: #project/devops 
Areas: #area/work
Subject: #subject/git
Type: #type/idea
Learning priority: #priority/P1 
Status: #status/learning 
Related: [[Knowledge-Base/Technology/CI-CD/CI-CD|CI-CD]]
Tutorial: [[Git - GitHub]]

---
# Git Cheat Sheet for Beginners

This guide contains only the most common Git commands for beginners.

---

# Configuration

View the configured username:

```bash
git config --get user.name
```

View the configured email:

```bash
git config --get user.email
```

View all Git configuration:

```bash
git config --list
```

Set the username:

```bash
git config --global user.name "Your Name"
```

Set the email:

```bash
git config --global user.email "your@email.com"
```

---

# Authentication

Git **does not have a login command**.

Authentication occurs automatically when you run a command that accesses the remote repository, such as:

```bash
git clone <repository-url>
```

or

```bash
git push
```

Depending on your configuration, authentication may use:

- Personal Access Token (PAT)
- SSH Key
- Git Credential Manager


---

# Clone a Repository

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/username/project.git
```

---

# Check Repository Status

```bash
git status
```

---

# Update Your Local Repository

Download and merge changes from the remote repository:

```bash
git pull
```

---

# View Branches

List local branches:

```bash
git branch
```

List local and remote branches:

```bash
git branch -a
```

---

# Switch Branches

```bash
git switch branch-name
```

Or (compatible with older Git versions):

```bash
git checkout branch-name
```

---

# Create a New Branch

```bash
git switch -c new-branch
```

Or:

```bash
git checkout -b new-branch
```

---

# Fetch Remote Changes

Download changes without merging them:

```bash
git fetch
```

---

# View Commit History

Short history:

```bash
git log --oneline
```

Full history:

```bash
git log
```

Last 5 commits:

```bash
git log -5
```

---

# View Changes

Show unstaged changes:

```bash
git diff
```

---

# Stage Files

Stage a single file:

```bash
git add file.txt
```

Stage all changes:

```bash
git add .
```

---

# Create a Commit

```bash
git commit -m "Describe your changes"
```

---

# Push Changes

```bash
git push
```

For the first push of a new branch:

```bash
git push -u origin branch-name
```

---

# Discard Local Changes

Discard changes in a specific file:

```bash
git restore file.txt
```

Discard all unstaged changes:

```bash
git restore .
```

---

# View Remote Repositories

```bash
git remote -v
```

---

# Show the Current Branch

```bash
git branch --show-current
```

---

# Quick Repository Summary

```bash
git status
```

This command shows:

- Current branch
- Modified files
- Staged files
- Untracked files

---

# Typical Daily Workflow

```bash
git pull
git status
git add .
git commit -m "My changes"
git push
```

---

# Quick Reference

|Action|Command|
|---|---|
|View username|`git config --get user.name`|
|View email|`git config --get user.email`|
|View configuration|`git config --list`|
|Clone repository|`git clone <repository-url>`|
|Check status|`git status`|
|Update repository|`git pull`|
|Fetch changes|`git fetch`|
|List branches|`git branch`|
|Switch branch|`git switch branch-name`|
|Create branch|`git switch -c new-branch`|
|Show current branch|`git branch --show-current`|
|View commits|`git log --oneline`|
|View changes|`git diff`|
|Stage all changes|`git add .`|
|Create commit|`git commit -m "message"`|
|Push changes|`git push`|
|View remotes|`git remote -v`|
|Discard local changes|`git restore .`|
