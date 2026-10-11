# Functions — Defaults, *args/**kwargs, Scope, Mutable Trap

Part 1 functions take fixed inputs. Real-world functions need flexibility — and scope discipline.

## Default Arguments: Sensible Fallbacks

```python
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("ada")
greet("grace", "Hi")
greet("linus", greeting="Welcome")
```

```text
Hello, ada!
Hi, grace!
Welcome, linus!
```

Rules: defaults go **last**, and they are evaluated **once** at definition time. That second fact is the source of the famous trap below.

---

## `*args`: Any Number of Positional Args

```python
def total(*numbers):
    """Return the sum of any number of values."""
    print(numbers)  # a tuple
    return sum(numbers)

print(total(1, 2, 3))
print(total())
```

```text
(1, 2, 3)
6
0
```

`*args` collects extra positionals into a tuple. Use it for genuinely open-ended inputs (`max`-like helpers, logging).

---

## `**kwargs`: Any Number of Named Args

```python
def profile(**info):
    """Print any named fields."""
    print(info)  # a dict
    for key, value in info.items():
        print(f"{key}: {value}")

profile(name="ada", born=1815)
```

```text
{'name': 'ada', 'born': 1815}
name: ada
born: 1815
```

| Syntax | Collects | You get |
|---|---|---|
| `*args` | Extra positionals | A `tuple` |
| `**kwargs` | Extra keywords | A `dict` |

You will read far more `*args/**kwargs` than you write — frameworks use them to forward arguments. Recognize the shape even if you rarely define it.

---

## Scope (LEGB): Where Names Are Looked Up

Python searches four levels, in order:

| Level | Name | Example |
|---|---|---|
| Local | Inside current function | `x = 1` in `def f():` |
| Enclosing | Outer function (closures) | Rare at this level |
| Global | Top of module | `MAX = 100` |
| Built-in | Python itself | `len`, `print` |

```python
name = "global ada"

def greet():
    name = "local grace"  # shadows the global — separate variable
    print(name)

greet()
print(name)
```

```text
local grace
global ada
```

Assignment inside a function creates a **local** by default. To rebind a global you need the `global` keyword — but the better answer is usually "do not; pass it as a parameter and `return` the result".

> Read scope errors inside-out: the traceback shows the innermost frame last. The name was looked up L → E → G → B and found nowhere.

---

## The Mutable Default Trap

This is the most famous Python gotcha. Do NOT do this:

```python
def add_item(item, cart=[]):  # DANGER: one shared list!
    cart.append(item)
    return cart

print(add_item("apple"))
print(add_item("pear"))  # pear joins apple's list?!
```

```text
['apple']
['apple', 'pear']
```

The default `[]` is created **once** when `def` runs, then shared across all calls. Fix with `None`:

```python
def add_item(item, cart=None):
    """Return cart with item appended; fresh list when omitted."""
    if cart is None:
        cart = []
    cart.append(item)
    return cart

print(add_item("apple"))
print(add_item("pear"))
```

```text
['apple']
['pear']
```

> Never use `[]` or `{}` as a default. Use `None` and build the fresh object inside. Linters flag this — listen to them.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Mutable default `[]` | State leaks between calls | Default `None`, create inside |
| `UnboundLocalError` | Assign + read same name in function | Pass as param / `return` instead of `global` |
| `*args` confusion | `total([1,2])` vs `total(1,2)` | `*` collects positionals; a list is ONE positional |
| Positional after keyword | `SyntaxError` | `f(1, b=2)` ok; `f(a=1, 2)` illegal |

---

## Checklist

- [ ] I can write defaults, `*args`, `**kwargs` and explain what each collects
- [ ] I can state LEGB order and why assignment creates a local
- [ ] I never use `[]`/`{}` as defaults — `None` + fresh object instead
- [ ] I prefer params + `return` over `global`

---
#### [Previous](01-part.md) | [Next](exercises.md)
