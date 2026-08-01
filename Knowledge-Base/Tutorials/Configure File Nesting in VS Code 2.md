# Configure File Nesting in VS Code

**File Nesting** allows you to group related files under a primary file in the Explorer, keeping your project structure cleaner and easier to navigate.

### 1. Open the JSON Settings

Press:

```text
Ctrl + Shift + P
```

Type:

```text
Preferences: Open User Settings (JSON)
```

---

### 2. Add the Configuration

Add the following block to your settings file:

```json
{
  "explorer.fileNesting.enabled": true,
  "explorer.fileNesting.expand": false,
  "explorer.fileNesting.patterns": {
    "package.json": "package-lock.json, yarn.lock, pnpm-lock.yaml",
    "*.go": "${capture}_test.go",
    "*.ts": "${capture}.js, ${capture}.map",
    "*.yaml": "${capture}.yml"
  }
}
```

---

### 3. Example

With the configuration:

```json
"explorer.fileNesting.patterns": {
  "*.go": "${capture}_test.go"
}
```

The following structure:

```text
main.go
main_test.go
```

will be displayed as:

```text
main.go
└── main_test.go
```

---

### 4. Apply the Changes

After saving the settings file, VS Code will usually apply the changes automatically. If not, run:

```text
Ctrl + Shift + P
Reload Window
```

---

### Tip

To view the default file nesting patterns provided by VS Code:

```text
Ctrl + Shift + P
Preferences: Open Default Settings (JSON)
```

Search for:

```text
explorer.fileNesting.patterns
```

This will show all built-in file nesting examples and patterns available in VS Code.