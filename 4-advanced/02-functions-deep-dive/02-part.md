# Closures and Decorators

A closure remembers variables from its enclosing scope. A decorator is a function that wraps another function.

> Master this sentence: a decorator takes a function, defines a wrapper closure, returns it.

## Closures

```python
def make_adder(n: int):
    def add(x: int) -> int:
        return x + n  # n remembered from enclosing scope
    return add

add5 = make_adder(5)
print(add5(10))
print(add5(20))
```

```text
15
25
```

`add5` is a closure: `add` plus the remembered `n=5`. Each call to `make_adder` creates independent memory.

```python
add10 = make_adder(10)
print(add10(10))
print(add5(10))
```

```text
20
15
```

| Concept | Meaning |
|---|---|
| Enclosing scope | Variables in outer `def` |
| Closure | Inner function + remembered variables |
| Factory | Outer function that builds closures |

Use closures for factories, counters, and specialized validators.

## Minimal Decorators

```python
from functools import wraps
import time

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

@timer
def slow_add(a: int, b: int) -> int:
    time.sleep(0.01)
    return a + b

print(slow_add(2, 3))
print(slow_add.__name__)
```

```text
slow_add took 0.0101s
5
slow_add
```

Without `@wraps`, `__name__` would be `"wrapper"` and docs would break. Always use it.

Check timing behavior:

```bash
python -c "import time; s=time.perf_counter(); time.sleep(0.01); print(f'{time.perf_counter()-s:.4f}s')"
```

```text
0.0101s
```

## `@lru_cache` Pattern

`functools.lru_cache` memoizes pure functions. Huge win for repeated expensive calls.

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n: int) -> int:
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

print(fib(10))
print(fib.cache_info())
```

```text
55
CacheInfo(hits=8, misses=11, maxsize=None, currsize=11)
```

Only cache pure functions (same input, same output, no side effects). Never cache functions that read files, time, or random.

| Decorator | Purpose | Remember |
|---|---|---|
| `@timer` (custom) | Measure time | Needs `functools.wraps` |
| `@lru_cache` | Memoize | Pure functions only |
| `@property` | Computed attribute | From classes module |
| Custom `@retry` / `@auth` | Wrap behavior | Same wrapper pattern |

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Missing `@wraps` | `__name__` is `wrapper`, help broken | Add `@wraps(func)` |
| Forgetting to return wrapper | Decorated function is `None` | `return wrapper`, not `return wrapper()` |
| Not returning `result` | Function returns `None` | `result = func(...); return result` |
| Caching impure function | Stale data | Only `@lru_cache` pure functions |
| Late binding in loop closures | All closures share last value | Bind with default arg: `lambda x, n=n: ...` |

## Checklist

- [ ] I can write a closure factory that remembers state
- [ ] I can write a decorator with `*args, **kwargs` + `return result`
- [ ] I always use `functools.wraps`
- [ ] I know when `@lru_cache` is safe
- [ ] I can read `@timer` stacked decorators top-down

#### [Previous](./01-part.md) | [Next](./exercises.md)
