# 32 – Type Hints in Python

This document explains **type hints** — annotations that describe what type of value a variable, parameter, or return value should be. FastAPI relies heavily on type hints to validate requests, generate documentation, and convert data automatically.

If you are new to Python, read [01-variables.md](01-variables.md), [03-list.md](03-list.md), and [19-class.md](19-class.md) first.

---

## 1. What Are Type Hints?

Type hints are optional labels you add to Python code. They do not change how Python runs your program by default, but tools and frameworks (including FastAPI) use them to understand your data.

```python
name: str = "abbas"
age: int = 25
```

```python
def greet(name: str) -> str:
    return f"Hello, {name}"
```

| Part | Meaning |
|------|---------|
| `name: str` | `name` should be a string |
| `age: int` | `age` should be an integer |
| `-> str` | the function returns a string |

---

## 2. Basic Types

| Hint | Python type | Example value |
|------|-------------|---------------|
| `str` | string | `"hello"` |
| `int` | integer | `42` |
| `float` | decimal number | `3.14` |
| `bool` | boolean | `True`, `False` |
| `bytes` | raw binary data | `b"data"` |

### Function Parameters

```python
def create_user(name: str, age: int):
    return {"name": name, "age": age}
```

When you call `create_user("ali", 25)`, Python knows `name` is expected to be a string and `age` an integer.

---

## 3. Collections

### Lists

```python
users: list[str] = ["abbas", "mmd"]
ages: list[int] = [20, 37, 19]
```

Older style (still common):

```python
from typing import List

users: List[str] = ["abbas", "mmd"]
```

### Dictionaries

```python
user: dict[str, int] = {"age": 25}
```

---

## 4. Optional Values

Sometimes a value can be missing. Use `Optional` or the union operator `|`.

### Python 3.10+ (recommended)

```python
def find_user(name: str | None = None):
    if name is None:
        return "all users"
    return f"user: {name}"
```

### Older style

```python
from typing import Optional

def find_user(name: Optional[str] = None):
    ...
```

`str | None` and `Optional[str]` mean the same thing: the value is either a string or `None`.

**Default values make parameters optional.** In FastAPI, a query parameter with a default is optional; one without a default is required.

---

## 5. Union Types

A value can be one of several types:

```python
identifier: int | str = 42
identifier = "user-abc"  # also valid
```

---

## 6. Annotated

`Annotated` attaches extra metadata to a type. FastAPI uses it to add validation rules (length limits, numeric ranges, descriptions).

```python
from typing import Annotated
from fastapi import Query

target_name: Annotated[str | None, Query(max_length=50)] = None
```

Read as: "this is a string or None, and FastAPI should treat it as a query parameter with a maximum length of 50."

You will see `Annotated` in FastAPI chapters on [query parameters](../libraries/fastapi/07-query-parameters.md) and [advanced parameters](../libraries/fastapi/10-query-path-advanced-params.md).

---

## 7. The Ellipsis (`...`)

In FastAPI parameter definitions, `...` means **required** (no default):

```python
from fastapi import Path

user_id: int = Path(...)
```

This is different from `None`. `...` tells the framework "the client must provide this value."

---

## 8. Why FastAPI Needs Type Hints

FastAPI reads your function signatures and uses type hints to:

| Behavior | Example |
|----------|---------|
| Validate input | Reject `age=abc` when `age: int` |
| Parse path segments | `/user/42` → `user_id: int = 42` |
| Read query strings | `?name=ali` → `name: str = "ali"` |
| Parse JSON bodies | JSON → Pydantic model instance |
| Generate `/docs` | Swagger UI shows types and rules |

Without type hints, you would write all of this validation manually.

---

## 9. Type Hints vs Runtime Types

Type hints are **not enforced** by Python itself at runtime:

```python
age: int = "not a number"  # Python allows this; linters will warn
```

FastAPI and Pydantic **do** enforce types for API input. Your own internal variables are only checked if you use a tool like `mypy` or if a framework validates them.

---

## Summary

* Type hints describe expected data types for variables, parameters, and return values.
* Basic hints: `str`, `int`, `float`, `bool`, `list`, `dict`.
* `str | None` or `Optional[str]` means the value can be missing.
* `Annotated` adds framework-specific metadata (used heavily in FastAPI).
* FastAPI uses type hints for validation, parsing, and automatic API documentation.
