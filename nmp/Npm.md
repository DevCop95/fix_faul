# ⚙️ Fix: npm Global Install Issues on Windows (Git Bash)

![Platform](https://img.shields.io/badge/platform-Windows-blue)
![Shell](https://img.shields.io/badge/shell-Git%20Bash-green)
![Node](https://img.shields.io/badge/node-%3E%3D18-brightgreen)
![npm](https://img.shields.io/badge/npm-%3E%3D8-red)
![Status](https://img.shields.io/badge/status-stable-success)

---

## 📌 Overview

This document describes a common issue when installing global npm packages on Windows using Git Bash, particularly with modern CLI tools such as:

* `@openai/codex`
* `@anthropic-ai/claude-code`
* other Node-based CLIs

---

## ⚠️ Problem

Running:

```bash
npm install -g <package>
```

Results in errors like:

```text
EEXIST: file already exists, mkdir 'C:\Users\USER\AppData\Roaming\npm'
```

Or inconsistent behavior:

* Global packages install but are not accessible
* Binaries are not generated (`command not found`)
* `npm config set prefix` is ignored
* `--prefix` does not apply correctly
* Mixed Windows/Unix paths break resolution

---

## 🧠 Root Cause

This issue is caused by a combination of:

* Windows-based npm global directory (`AppData\Roaming\npm`)
* Git Bash using Unix-style paths (`/c/...`)
* Conflicts between:

  * `.npmrc`
  * environment variables
  * PATH resolution
* Previous corrupted installations (e.g., `npm` as file instead of directory)
* Partial or inconsistent CLI support on Windows

---

## 🔍 Symptoms

| Issue               | Description                           |
| ------------------- | ------------------------------------- |
| `EEXIST`            | npm cannot create global directory    |
| `command not found` | CLI binary not available              |
| Silent installs     | Package installs but no usable output |
| Ignored prefix      | npm ignores custom global path        |
| Broken PATH         | Mixed Windows/Linux paths             |

---

## ❌ Non-reliable Fixes

The following approaches may fail on Windows:

```bash
npm config set prefix ~/.npm-global
```

```bash
npm install -g <package> --prefix ~/.npm-global
```

```bash
export PATH=...
```

These are often overridden by Windows npm configuration.

---

## ✅ Recommended Solution

### ✔️ Use `npx` (no global install)

```bash
npx <package>
```

Example:

```bash
npx @openai/codex
```

---

## 🚀 Usage in Specific Directory

```bash
cd /c/Users/USER/Desktop/vscode/Devops
npx @openai/codex
```

✔️ Runs the CLI in the current working directory
✔️ No dependency on global npm configuration

---

## 🛠 Optional: Alias for Convenience

```bash
echo 'alias codex-devops="cd /c/Users/USER/Desktop/vscode/Devops && npx @openai/codex"' >> ~/.bashrc
source ~/.bashrc
```

Usage:

```bash
codex-devops
```

---

## 🧩 Alternative Approaches

### 🟢 Option A — Git Bash + npx (Recommended)

* Simple
* No global install issues
* Works immediately

### 🔵 Option B — WSL (Professional Setup)

* Native Linux environment
* Full compatibility
* Recommended for serious development

---

## ⚠️ Compatibility Notes

| Environment        | Status                    |
| ------------------ | ------------------------- |
| Windows + Git Bash | ⚠️ Partial / inconsistent |
| Windows (native)   | ⚠️ Limited                |
| WSL (Ubuntu)       | ✅ Recommended             |
| Linux/macOS        | ✅ Fully supported         |

---

## 🎯 Conclusion

Global npm installations in Windows (especially with Git Bash) are unreliable due to path and configuration conflicts.

👉 The most stable approach is:

```bash
npx <package>
```

---

## 🧾 TL;DR

```bash
cd /your/project/path
npx @openai/codex
```

---

## 📚 Notes

* This is an environment limitation, not a user error
* Modern CLI tools assume Unix-like environments
* Prefer Linux/WSL for production workflows

---

## 🧑‍💻 Author Notes

If you spent hours debugging this:

Welcome to cross-platform Node.js tooling 😄
