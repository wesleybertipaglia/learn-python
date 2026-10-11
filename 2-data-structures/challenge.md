# Challenge — Contact Book CLI

Build a menu-driven contact book using a list of dicts. This ties together lists, strings, dicts, sets, and comprehensions.

## 1. Requirements

Store contacts as a list of dicts:

```python
contacts = [
    {"name": "Ada Lovelace", "phone": "555-0101", "email": "ada@example.com"},
]
```

Support:

| # | Command | Behavior |
|---|---|---|
| 1 | Add | Validate non-empty name, unique email |
| 2 | Search | Case-insensitive substring on name/email |
| 3 | List | Sorted by name, numbered |
| 4 | Remove | By exact email, confirm |
| 5 | Save/Load | JSON file `contacts.json` (optional but recommended) |
| 0 | Quit | Exit loop |

> Validate everything the user types: empty names, bad emails, and missing files must not crash the app.

---

## 2. Starter structure

```python
import json
from pathlib import Path

DB = Path("contacts.json")
contacts: list[dict] = []

def load() -> None:
    if DB.exists():
        contacts.extend(json.loads(DB.read_text()))

def save() -> None:
    DB.write_text(json.dumps(contacts, indent=2))

def list_contacts() -> None:
    for i, c in enumerate(sorted(contacts, key=lambda x: x["name"].lower()), 1):
        print(f"{i}. {c['name']} — {c['phone']} — {c['email']}")

def find(query: str) -> list[dict]:
    q = query.strip().lower()
    return [c for c in contacts if q in c["name"].lower() or q in c["email"].lower()]
```

```bash
python contacts.py
```

```text
1. Ada Lovelace — 555-0101 — ada@example.com
2. Grace Hopper — 555-0102 — grace@example.com
```

---

## 3. Menu loop

```python
def menu() -> None:
    load()
    while True:
        print("\n1 add | 2 search | 3 list | 4 remove | 5 save | 0 quit")
        choice = input("> ").strip()
        if choice == "1":
            add_contact()
        elif choice == "2":
            q = input("search: ")
            for c in find(q):
                print(c)
        elif choice == "3":
            list_contacts()
        elif choice == "4":
            remove_contact()
        elif choice == "5":
            save()
            print("saved")
        elif choice == "0":
            print("bye")
            break
        else:
            print("unknown option")
```

---

## 4. Add with validation

```python
def add_contact() -> None:
    name = input("name: ").strip()
    phone = input("phone: ").strip()
    email = input("email: ").strip().lower()
    if not name:
        print("name required")
        return
    if "@" not in email or "." not in email:
        print("invalid email")
        return
    if any(c["email"] == email for c in contacts):
        print("email already exists")
        return
    contacts.append({"name": name, "phone": phone, "email": email})
    print(f"added {name}")
```

> Normalize emails with `strip().lower()` and compare case-insensitively, or `Ada@X.com` and `ada@x.com` become duplicates.

---

## 5. Remove by email

```python
def remove_contact() -> None:
    email = input("email to remove: ").strip().lower()
    for i, c in enumerate(contacts):
        if c["email"] == email:
            confirm = input(f"delete {c['name']}? [y/N]: ").strip().lower()
            if confirm == "y":
                contacts.pop(i)
                print("removed")
            return
    print("not found")
```

---

## 6. Save / load JSON

```python
import json
print(json.dumps(contacts, indent=2))
```

```text
[
  {
    "name": "Ada Lovelace",
    "phone": "555-0101",
    "email": "ada@example.com"
  }
]
```

Handle corrupt files gracefully:

```python
try:
    data = json.loads(DB.read_text())
except (FileNotFoundError, json.JSONDecodeError):
    data = []
```

---

## 7. Example transcript

```text
1 add | 2 search | 3 list | 4 remove | 5 save | 0 quit
> 1
name: Ada Lovelace
phone: 555-0101
email: ada@example.com
added Ada Lovelace

> 1
name: grace hopper
phone: 555-0102
email: GRACE@example.com
added grace hopper

> 3
1. Ada Lovelace — 555-0101 — ada@example.com
2. grace hopper — 555-0102 — grace@example.com

> 2
search: ADA
{'name': 'Ada Lovelace', 'phone': '555-0101', 'email': 'ada@example.com'}

> 4
email to remove: grace@example.com
delete grace hopper? [y/N]: y
removed

> 5
saved

> 0
bye
```

Run it:

```bash
python contacts.py
ls -l contacts.json
cat contacts.json
```

---

## 8. Tips

- Keep contacts sorted only for display — never rely on list position as ID; email is the key.
- Use a set of emails `{c["email"] for c in contacts}` for O(1) duplicate checks.
- Use `Counter` to report most common email domains as an extra.
- Use `deque` only if you add an undo stack — otherwise a list is fine.

---

## Common Bugs

| Bug | Symptom | Fix |
|---|---|---|
| `contacts["email"]` | `TypeError` — list indexed by str | Loop or index `by_email` dict |
| Case-sensitive search | `ADA` finds nothing | Lower both sides |
| Duplicate emails | Two same contacts | Check `any(... email == ...)` |
| Crash on missing JSON | `FileNotFoundError` | `if DB.exists()` + `try/except` |
| `input()` whitespace | `" ada " != "ada"` | Always `.strip()` |
| Saving tuple/set | `TypeError` from `json` | Store plain list of dicts with str values |

## Checklist (Grading)

- [ ] Contacts stored as `list[dict]` with `name`, `phone`, `email`
- [ ] Add validates name non-empty and email shape + uniqueness
- [ ] Search is case-insensitive substring over name and email
- [ ] List prints sorted by name with numbers
- [ ] Remove finds by exact email (lowercased) with confirmation
- [ ] Menu loops until `0`, handles unknown options without crash
- [ ] Save/load JSON round-trips (optional: handle corrupt file)
- [ ] No `KeyError` / `IndexError` on bad input; uses `.get()` / `in` / `try`

## Extras (pick 1–2)

- [ ] Edit phone by email
- [ ] Top email domains with `Counter(c["email"].split("@")[1] for c in contacts)`
- [ ] Undo last remove with a `deleted: list` stack (`append`/`pop`)
- [ ] Export CSV with `",".join(...)` per row
- [ ] Unit-test `find()` and `add` validation with `pytest`

#### [Previous](06-comprehensions-collections/exercises.md) | [Next](../3-oop/01-classes/01-part.md)
