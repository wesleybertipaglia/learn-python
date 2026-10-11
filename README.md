# Python

Python is a high-level, general-purpose language created by Guido van Rossum and first released in 1991. It is used in web development, data science, scripting, automation, and AI.

- Interpreted: runs via the CPython interpreter, no compile step.
- Dynamically typed: types are checked at runtime.
- Garbage collected: memory is managed automatically.
- Batteries-included: rich standard library out of the box.
- Multi-paradigm: procedural, OOP, and functional styles.
- Indentation-based: blocks are defined by indentation, not braces.

## Why learn Python?

- Readability: code reads close to plain English.
- Fastest way to ship ideas: prototype to production quickly.
- Huge ecosystem: install anything with `pip`.
- Gateway to backend, data, AI, and automation with one language.

## What you need to know before starting

Nothing. No prior programming experience is required. The goal of this course is to make you a programmer who knows Python — from zero to writing tested, idiomatic code.

> **Warning:** Python won't stop you from shadowing builtins, mutating shared defaults, or catching everything with a bare `except`. This course teaches you how to avoid those traps.

## How to use this course

Each topic has `01-part.md`, `02-part.md`, and `exercises.md`. Each module ends with `challenge.md`. Examples assume Python 3.12+ and run with `python`.

```bash
python --version  # need 3.12+
python hello.py
```

## Topics

### 1. Fundamentals

1. [Getting Started](./1-fundamentals/01-getting-started/01-part.md)
2. [Syntax](./1-fundamentals/02-syntax/01-part.md)
3. [Data Types](./1-fundamentals/03-data-types/01-part.md)
4. [Operators](./1-fundamentals/04-operators/01-part.md)
5. [Conditionals](./1-fundamentals/05-conditionals/01-part.md)
6. [Loops](./1-fundamentals/06-loops/01-part.md)
7. [Functions](./1-fundamentals/07-functions/01-part.md)
8. [Exceptions](./1-fundamentals/08-exceptions/01-part.md)
9. [Debugging](./1-fundamentals/09-debugging/01-part.md)

> [Final challenge of the module](./1-fundamentals/challenge.md)

### 2. Data Structures

1. [Lists](./2-data-structures/01-lists/01-part.md)
2. [Tuples](./2-data-structures/02-tuples/01-part.md)
3. [Strings](./2-data-structures/03-strings/01-part.md)
4. [Dicts](./2-data-structures/04-dicts/01-part.md)
5. [Sets](./2-data-structures/05-sets/01-part.md)
6. [Comprehensions and Collections](./2-data-structures/06-comprehensions-collections/01-part.md)

> [Final challenge of the module](./2-data-structures/challenge.md)

### 3. Object-Oriented Programming

Classes, objects, and the four pillars: encapsulation, inheritance, polymorphism, and abstraction.

1. [Classes](./3-oop/01-classes/01-part.md)
2. [Objects](./3-oop/02-objects/01-part.md)
3. [Encapsulation](./3-oop/03-encapsulation/01-part.md)
4. [Inheritance](./3-oop/04-inheritance/01-part.md)
5. [Polymorphism](./3-oop/05-polymorphism/01-part.md)
6. [Abstraction](./3-oop/06-abstraction/01-part.md)

> [Final challenge of the module](./3-oop/challenge.md)

### 4. Advanced Python

Beyond the basics: iterators, decorators, files, and modules.

1. [Iterators and Generators](./4-advanced/01-iterators-generators/01-part.md)
2. [Functions Deep Dive (closures, decorators, lambdas)](./4-advanced/02-functions-deep-dive/01-part.md)
3. [File I/O (pathlib, json, csv)](./4-advanced/03-file-io/01-part.md)
4. [Modules, Venv and Pip](./4-advanced/04-modules-venv-pip/01-part.md)

> [Final challenge of the module](./4-advanced/challenge.md)

### 5. Testing

1. [Introduction](./5-testing/01-introduction/01-part.md)
2. [Pytest](./5-testing/02-pytest/01-part.md)
3. [Tooling (unittest, coverage, ruff, mypy)](./5-testing/03-tooling/01-part.md)

> [Final challenge of the module](./5-testing/challenge.md)

## Warnings are not noise

Treat warnings as bugs. Run with warnings as errors and lint every change:

```bash
python --version
python -W error app.py
pip install -r requirements.txt
python -m pytest
ruff check .
```

```text
Warning: never commit .venv/ or __pycache__/ — that is what .gitignore is for.
```

## Contributing

Found a typo or a broken example? Open a PR with a minimal fix and a test or snippet proving it. Keep lessons short, runnable, and Python 3.12+ compatible.
