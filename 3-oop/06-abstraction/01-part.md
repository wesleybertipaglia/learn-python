# Abstraction: ABCs and Abstract Methods

Abstraction hides how and shows what. Abstract base classes let you declare "every exporter must implement `export()`" and have Python enforce it.

> Program to an interface, not an implementation. The ABC is the contract.

## `abc.ABC` and `@abstractmethod`

An abstract method has no usable body. A class with one cannot be instantiated until children fill every gap.

```python
from abc import ABC, abstractmethod

class Exporter(ABC):
    @abstractmethod
    def export(self, rows: list[dict]) -> str:
        ...

class CsvExporter(Exporter):
    def export(self, rows: list[dict]) -> str:
        lines = [",".join(r.keys())]
        lines += [",".join(str(v) for v in r.values()) for r in rows]
        return "\n".join(lines)

print(CsvExporter().export([{"a": 1, "b": 2}]))
```

```text
a,b
1,2
```

Try to instantiate the ABC itself and Python stops you:

```python
from abc import ABC, abstractmethod

class Exporter(ABC):
    @abstractmethod
    def export(self, rows: list[dict]) -> str:
        ...

try:
    Exporter()
except TypeError as e:
    print(f"blocked: {e}")
```

```text
blocked: Can't instantiate abstract class Exporter without an implementation for abstract method 'export'
```

| Rule | Meaning |
|---|---|
| Subclass must implement all `@abstractmethod`s | Else it stays abstract too |
| ABC cannot be instantiated | Enforced at runtime |
| Concrete methods can live on the ABC | Shared helpers for all children |

Show the block from the shell:

```bash
python -c "from abc import ABC, abstractmethod
class E(ABC):
    @abstractmethod
    def go(self): ...
try: E()
except TypeError as e: print('blocked OK')"
```

```text
blocked OK
```

## Interface vs Implementation

The ABC names what. Each subclass decides how.

```python
from abc import ABC, abstractmethod
import json

class Exporter(ABC):
    @abstractmethod
    def export(self, rows: list[dict]) -> str:
        ...

    def preview(self, rows: list[dict]) -> str:
        return self.export(rows)[:60]

class JsonExporter(Exporter):
    def export(self, rows: list[dict]) -> str:
        return json.dumps(rows, indent=2)

class CsvExporter(Exporter):
    def export(self, rows: list[dict]) -> str:
        lines = [",".join(rows[0].keys())]
        lines += [",".join(str(v) for v in r.values()) for r in rows]
        return "\n".join(lines)

rows = [{"name": "ana", "balance": 70.0}]
for exp in [JsonExporter(), CsvExporter()]:
    print(type(exp).__name__)
    print(exp.preview(rows))
```

```text
JsonExporter
[
  {
    "name": "ana",
    "balance": 70.0
  }
CsvExporter
name,balance
ana,70.0
```

Callers take `Exporter` and never care which subclass arrives. Swap JSON for CSV with a one-line change.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Instantiating the ABC | `TypeError: Can't instantiate abstract class` | Instantiate a concrete subclass |
| Partial implementation | Child still abstract, same `TypeError` | Implement every `@abstractmethod` |
| Abstract method with real logic ignored | Duplicated code in children | Put shared logic in concrete ABC methods |
| Over-abstracting early | Three ABCs for two classes | Start concrete, extract ABC on second use |
| Signature drift in child | Checker error, runtime surprise | Copy the abstract signature exactly |

## Checklist

- [ ] I can declare an ABC with `@abstractmethod`
- [ ] I can explain why the ABC cannot be instantiated
- [ ] My subclasses implement every abstract method
- [ ] Shared helpers live on the ABC as concrete methods
- [ ] Callers depend on the ABC, not the subclass

#### [Previous](../05-polymorphism/exercises.md) | [Next](./02-part.md)
