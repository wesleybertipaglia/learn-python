# Template Method and When Abstraction Pays

The template method puts the skeleton on the ABC and lets children fill in the steps. Abstraction pays when three callers share one contract — not before.

> Let the parent own the algorithm. Let children own the details.

## Template-Method Pattern

The ABC defines the order of operations. Subclasses override only the varying step.

```python
from abc import ABC, abstractmethod

class StatementExporter(ABC):
    def export_statement(self, owner: str, balance: float) -> str:
        header = self.header(owner)
        body = self.body(balance)
        return f"{header}\n{body}\n-- end --"

    @abstractmethod
    def header(self, owner: str) -> str:
        ...

    @abstractmethod
    def body(self, balance: float) -> str:
        ...

class TextStatement(StatementExporter):
    def header(self, owner: str) -> str:
        return f"Statement for {owner}"

    def body(self, balance: float) -> str:
        return f"Balance: {balance:.2f} EUR"

class HtmlStatement(StatementExporter):
    def header(self, owner: str) -> str:
        return f"<h1>{owner}</h1>"

    def body(self, balance: float) -> str:
        return f"<p>{balance:.2f} EUR</p>"

print(TextStatement().export_statement("ana", 70.0))
print(HtmlStatement().export_statement("ana", 70.0))
```

```text
Statement for ana
Balance: 70.00 EUR
-- end --
<h1>ana</h1>
<p>70.00 EUR</p>
-- end --
```

| Piece | Lives on | Changes? |
|---|---|---|
| `export_statement` template | ABC, concrete | Never per subclass |
| `header` / `body` steps | Subclasses | Each format differs |
| Footer `"-- end --"` | ABC | Uniform for all |

## When Abstraction Pays Off

| Situation | Verdict |
|---|---|
| One exporter, no second in sight | Skip the ABC, write the class |
| Two exporters sharing a signature | Extract a `Protocol` or ABC |
| Three+ variants + shared skeleton | Template method on an ABC |
| Framework used by other teams | ABC documents and enforces the contract |
| Unsure | Wait for the second concrete use case |

Abstraction has a cost: indirection. Every ABC should remove more confusion than it adds.

```python
from abc import ABC, abstractmethod

class Notifier(ABC):
    @abstractmethod
    def send(self, msg: str) -> None:
        ...

class PrintNotifier(Notifier):
    def send(self, msg: str) -> None:
        print(f"notify: {msg}")

def alert(users: list[str], notifier: Notifier) -> None:
    for u in users:
        notifier.send(f"hi {u}")

alert(["ana", "bo"], PrintNotifier())
```

```text
notify: hi ana
notify: hi bo
```

`alert` depends on `Notifier`, so swapping in an email or SMS sender touches nothing else.

Prove the swap is one line:

```bash
python -c "print('alert(users, EmailNotifier())  # nothing else changes')"
```

```text
alert(users, EmailNotifier())  # nothing else changes
```

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| Template calls overridable `__init__` steps | Half-built object | Keep template to methods, init stays simple |
| Subclass changes template signature | Substitution breaks | Never override the template itself |
| Premature ABC for one class | Extra file, no benefit | Inline until the second variant appears |
| God ABC with 10 abstract methods | Painful subclasses | Split into small single-purpose ABCs |
| Forgetting `@abstractmethod` | Incomplete child instantiates fine | Mark every required step abstract |

## Checklist

- [ ] I can write a template method with abstract steps
- [ ] Subclasses vary steps without touching the skeleton
- [ ] I can name the cost of an abstraction
- [ ] I introduce ABCs on the second use case, not the first
- [ ] Callers depend on the abstraction, not the detail
- [ ] My ABCs stay small with one clear responsibility

#### [Previous](./01-part.md) | [Next](./exercises.md)
