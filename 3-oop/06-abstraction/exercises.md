# Exercises: Abstraction

> Declare the contract once. Plug in many implementations.

## Task 1: Exporter ABC

Write an `Exporter` ABC with abstract `export(rows)`. `CsvExporter` implements it; `Exporter()` raises `TypeError`.

```python
exp = None  # CsvExporter().export([{"a": 1}])
```

```text
Expected: header a, row 1
```

## Task 2: Template Statement

Add a `StatementExporter` template with abstract `header`/`body` and a concrete `export_statement`.

```python
out = None  # TextStatement().export_statement("ana", 70.0)
```

```text
Expected: contains "ana" and "70.00"
```

Verify the abstract block:

```bash
python -c "from solution import Exporter; Exporter()" && echo FAIL || echo "blocked OK"
```

```text
blocked OK
```

| Exercise | Focus | Hint |
|---|---|---|
| Exporter | ABC + abstract | Subclass must fill `export` |
| Template | Skeleton + steps | Parent owns order, child owns detail |
| Notifier | Swap impls | Caller takes the ABC type |

## Task 3: Notifier Swap

Write `PrintNotifier(Notifier)` and an `alert()` function that takes any `Notifier`.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Instantiating ABC | `TypeError` | Use concrete subclass |
| Missing step | Child still abstract | Implement all abstract methods |
| Overriding template | Broken substitution | Override steps only |

## Checklist

- [ ] ABC blocks direct instantiation
- [ ] Template output matches for each subclass
- [ ] Caller swaps implementations freely

#### [Previous](./02-part.md) | [Next](../../4-advanced/01-iterators-generators/01-part.md)
