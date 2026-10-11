# Getting Started — Install Python 3.12

Python is an interpreted language: you install one program (`python`) and it runs your `.py` files directly. No compiler, no build step.

This lesson gets you from zero to running code in under 15 minutes.

## Why Python 3.12+

This course targets Python 3.12 and newer. Older versions (3.8, 3.9) still work for most code, but 3.12 gives you:

| Feature | Why it matters |
|---|---|
| Better error messages | Tells you exactly what you did wrong |
| Faster interpreter | ~10% faster than 3.11 |
| `match` statement | Clean alternative to long `if/elif` chains (lesson 05) |
| Improved f-strings | You can nest quotes and expressions (lesson 03) |

> Always use Python 3. Never use Python 2 — it has been dead since 2020 and half its syntax will confuse you.

---

## Step 1 — Install on Linux

Most Linux distros ship Python, but it may be old. Check first:

```bash
python3 --version
```

If you see `Python 3.12.x` or `3.13.x`, you are done. If it is older, install fresh:

```bash
# Ubuntu / Debian
sudo apt update && sudo apt install python3.12
```

```bash
# Fedora
sudo dnf install python3.12
```

```bash
# Arch
sudo pacman -S python
```

Verify:

```bash
python3 --version
```

```text
Python 3.12.3
```

---

## Step 2 — Install on macOS

macOS ships a system Python you should **not** touch. Install your own:

```bash
# With Homebrew (recommended)
brew install python@3.12
```

Or download the installer from `python.org` → Downloads → macOS → Python 3.12.

Verify:

```bash
python3 --version
```

```text
Python 3.12.4
```

> On macOS and Linux the command is `python3`, not `python`. On Windows it is `python`. This course writes `python` for short — mentally swap in `python3` if you are on Linux/macOS.

---

## Step 3 — Install on Windows

1. Go to `python.org` → Downloads → Windows → Python 3.12 installer.
2. Run it. **Check the box "Add python.exe to PATH"** — this is the step everyone forgets.
3. Click "Install Now".

Verify in PowerShell or `cmd`:

```bash
python --version
```

```text
Python 3.12.4
```

If you get `'python' is not recognized`, reinstall and check that PATH box, then close and reopen your terminal.

---

## Step 4 — VS Code + Python Extension

You can use any editor, but this course assumes VS Code:

1. Install VS Code from `code.visualstudio.com`.
2. Open it → Extensions tab (Ctrl+Shift+X) → search `Python` → install the one by Microsoft.
3. Open a folder for this course: File → Open Folder → pick `learn-python/`.

Why the extension matters:

| Without it | With it |
|---|---|
| No autocomplete | Autocomplete + hover docs |
| No error squiggles | Red squiggles before you run |
| Run by hand in terminal | Green "Run" button + debugger |

Create a test file `hello.py`:

```python
print("hello from vscode")
```

```text
hello from vscode
```

---

## Step 5 — The Terminal in 60 Seconds

You only need four commands for now:

| Command | What it does | Example |
|---|---|---|
| `pwd` / `cd` | Show / change directory | `cd learn-python` |
| `ls` / `dir` | List files | `ls` |
| `python hello.py` | Run a Python file | `python hello.py` |
| `code .` | Open current folder in VS Code | `code .` |

Try it:

```bash
mkdir learn-python
cd learn-python
code hello.py
python hello.py
```

```text
hello from vscode
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `python: command not found` | Linux/macOS without alias | Use `python3` instead of `python` |
| `'python' is not recognized` | Windows PATH missing | Reinstall with "Add to PATH" checked |
| VS Code runs old Python | Wrong interpreter selected | Ctrl+Shift+P → "Python: Select Interpreter" → pick 3.12 |
| `Python 2.7.x` in version output | Ancient system Python | Install 3.12 separately, never replace system Python |

---

## Checklist

- [ ] `python --version` (or `python3 --version`) prints 3.12+
- [ ] VS Code with Microsoft Python extension installed
- [ ] You can create a file and run it with `python hello.py`
- [ ] You know how to open a terminal in your project folder

---
#### [Previous](../../README.md) | [Next](02-part.md)
