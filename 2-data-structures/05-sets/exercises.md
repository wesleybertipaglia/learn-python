# Sets — Exercises

Practice uniqueness, ops, and dedup.

## Setup

```bash
mkdir -p sets_ex && cd sets_ex
python --version
```

```python
print({1, 2, 2})
```

```text
{1, 2}
```

| Exercise | Focus | Hint |
|---|---|---|
| 1 | Creation | `set()` not `{}` |
| 2 | add/discard | Safe delete |
| 3 | Ops | `&`, `\|`, `-` |
| 4 | Dedup | `dict.fromkeys` |
| 5 | Speed | `in` timing |

> Sets are unordered — print with `sorted()` when output must be stable.

---

## 1. Empty trap
Show `type({})` vs `type(set())`. Build an empty set, `add` three items (one duplicate), print result.

## 2. Safe removal
From `{1, 2, 3}`, `discard(99)` then `remove(2)`. Wrap a second `remove(99)` in `try/except KeyError` and print the error.

## 3. Class overlap
Given `python = {"ada","grace"}` and `rust = {"grace","linus"}`, print union, intersection, difference both ways, symmetric difference.

## 4. Order-preserving dedup
Dedup `["b","a","b","c","a"]` three ways: `set`, `dict.fromkeys`, manual `seen` loop. Print all and explain order differences.

## 5. Lookup race
Build `list(range(20000))` and matching set. Time `19999 in list` vs `in set` with `time.perf_counter`. Print both timings.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `{}` empty | Wrong type | `set()` |
| `s[0]` | `TypeError` | `sorted(s)[0]` |
| `remove` missing | `KeyError` | `discard` |
| Op with list | `TypeError` | `set(lst)` first |
| Order assumed | Flaky | `sorted()` |

## Checklist

- [ ] Empty set created correctly
- [ ] Safe vs strict removal shown
- [ ] All 4 set ops printed
- [ ] Dedup keeps order once
- [ ] Timing proves set faster

#### [Previous](02-part.md) | [Next](../06-comprehensions-collections/01-part.md)
