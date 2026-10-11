# Debugging — breakpoint(), Print Discipline, ruff

Reading tracebacks diagnoses crashes. These three tools catch bugs that do NOT crash — wrong values, silent logic errors.

## `breakpoint()`: Freeze Time and Look Around

Drop `breakpoint()` where behavior confuses you. Python stops and gives you a debugger prompt (`pdb`):

```python
def total(prices):
    acc = 0
    for p in prices:
        breakpoint()   # execution pauses here each lap
        acc += p
    return acc

print(total([10, 20, 30]))
```

```text
> /home/user/demo.py(5)total()
-> acc += p
(Pdb) p acc
0
(Pdb) p p
10
(Pdb) c
10
60
```

Essential `pdb` commands:

| Command | Does |
|---|---|
| `p expr` | Print value (`p acc`, `p type(x)`) |
| `n` | Next line (step over) |
| `c` | Continue running |
| `q` | Quit the program |

> Remove every `breakpoint()` before you consider the program done. A forgotten breakpoint in a shipped script freezes it for every user. Search your file for the word before submitting.

---

## Print Debugging — With Discipline

`print()` debugging is fine. Spray-and-pray `print("here")` everywhere is not. Three rules:

```python
def withdraw(balance, amount):
    print(f"DEBUG withdraw: balance={balance!r} amount={amount!r}")
    if amount <= 0 or amount > balance:
        print("DEBUG withdraw: rejected")
        return None
    result = balance - amount
    print(f"DEBUG withdraw: result={result!r}")
    return result
```

1. **Label every debug print** (`DEBUG withdraw:`) so you can find and delete them.
2. **Print types, not just values**: `{x!r}` shows `'30'` vs `30`.
3. **Print at boundaries** (function entry/exit), not on every line.

| Disciplined | Undisciplined |
|---|---|
| `print(f"DEBUG f: x={x!r}")` | `print(x)`, `print("here")`, `print("!!!")` |
| Deleted before commit | Left in, confusing users |
| Shows repr + location | Shows bare value, origin unknown |

When the bug is found, delete the prints. If you need them again, `breakpoint()` is one line away.

---

## `ruff check`: a Second Pair of Eyes

[Ruff](https://docs.astral.sh/ruff/) is a fast Python linter. It catches unused imports, undefined names, and style violations before you run:

```bash
pip install ruff
ruff check demo.py
```

```python
# demo.py
import os  # unused

user_nmae = "ada"
print(user_name)
```

```text
demo.py:1:8: F401 `os` imported but unused
demo.py:4:7: F821 Undefined name `user_name`
Found 2 errors.
```

| Code | Meaning |
|---|---|
| `F401` | Unused import — delete it |
| `F821` | Undefined name — typo or missing definition |
| `E711` | `== None` — use `is None` |
| `B006` | Mutable default argument (lesson 07's trap!) |

Add it to your workflow: write → `ruff check` → run → test. Thirty seconds that prevents half the "it works on my machine" bugs.

> Linters advise; tests decide. `ruff check` clean does NOT mean correct — it means no *obvious* mistakes. You still need to run the program.

---

## The Debugging Loop

1. **Reproduce** — smallest input that triggers it.
2. **Read** — traceback bottom-up (lesson 09 part 1).
3. **Inspect** — `breakpoint()` or labeled prints at the boundary.
4. **Fix one thing** — then re-run immediately.
5. **Lint** — `ruff check` before you call it done.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Forgotten `breakpoint()` | Program hangs waiting at `(Pdb)` | Search and delete before finishing |
| Unlabeled prints left in | Users see `here`, `30`, `!!!` | `DEBUG name:` prefix, delete after |
| Ignoring `ruff` F821 | Runtime `NameError` you were warned about | Fix every F-code before running |
| Fixing two things at once | Cannot tell which change worked | One fix → re-run → repeat |

---

## Checklist

- [ ] I can pause with `breakpoint()` and use `p` / `n` / `c` / `q`
- [ ] My debug prints are labeled, show `{x!r}`, and get deleted after
- [ ] `ruff check` runs clean on my exercise files
- [ ] I follow reproduce → read → inspect → fix-one-thing → lint

---
#### [Previous](01-part.md) | [Next](exercises.md)
