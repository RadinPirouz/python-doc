# 14 – Pydantic Basics

This document explains **Pydantic** — a library for defining data models with automatic validation. FastAPI uses Pydantic to parse JSON request bodies, validate input, and shape API responses.

**Prerequisites:** [05-oop.md](05-oop.md) (classes), [10-type-hints.md](10-type-hints.md) (type hints), [11-json.md](11-json.md) (JSON).

Install Pydantic (included when you install FastAPI):

```bash
pip install pydantic
```

---

## 1. Why Pydantic?

Without Pydantic, you manually check every field:

```python
def create_user(data: dict):
    if "name" not in data:
        raise ValueError("name required")
    if not isinstance(data.get("age"), int):
        raise ValueError("age must be int")
```

With Pydantic, you declare the shape once:

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
```

Pydantic validates automatically and returns clear error messages.

---

## 2. Defining a Model

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
```

This class describes data that looks like:

```json
{
  "name": "ali",
  "age": 25
}
```

| Field | Type | Required |
|-------|------|----------|
| `name` | string | yes |
| `age` | integer | yes |

---

## 3. Creating Instances

### From keyword arguments

```python
user = User(name="ali", age=25)
print(user.name)  # ali
print(user.age)   # 25
```

### From a dictionary (like parsed JSON)

```python
data = {"name": "ali", "age": 25}
user = User(**data)
```

### From JSON string

```python
import json
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

text = '{"name": "ali", "age": 25}'
user = User.model_validate_json(text)
```

---

## 4. Validation Errors

Invalid data raises a validation error:

```python
User(name="ali", age="not a number")  # ValidationError
```

FastAPI catches these and returns HTTP **422 Unprocessable Entity** with details:

```json
{
  "detail": [
    {
      "type": "int_parsing",
      "loc": ["body", "age"],
      "msg": "Input should be a valid integer",
      "input": "not a number"
    }
  ]
}
```

---

## 5. Optional Fields and Defaults

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int = 18
    email: str | None = None
```

| Field | Behavior |
|-------|----------|
| `name` | Required |
| `age` | Optional; defaults to `18` |
| `email` | Optional; defaults to `None` |

---

## 6. Converting Back to Dict or JSON

```python
user = User(name="ali", age=25)

user.model_dump()       # {'name': 'ali', 'age': 25}
user.model_dump_json()  # '{"name":"ali","age":25}'
```

Older Pydantic v1 used `.dict()` and `.json()` — use `model_dump()` in Pydantic v2.

---

## 7. Separate Input and Output Models

A common API pattern: accept a password on input but never return it:

```python
class UserIn(BaseModel):
    username: str
    password: str

class UserOut(BaseModel):
    username: str
```

The client sends `UserIn`. The server responds with `UserOut`. The password stays server-side.

FastAPI supports this with `response_model=UserOut` on the route decorator. See [FastAPI responses](../libraries/fastapi/09-responses.md).

---

## 8. Pydantic in FastAPI

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    age: int

@app.post("/users")
def create_user(user: User):
    return user
```

When a client sends:

```http
POST /users
Content-Type: application/json

{"name": "ali", "age": 25}
```

FastAPI:

1. Reads the JSON body
2. Validates it against `User`
3. Passes a `User` instance to `create_user`
4. Serializes the return value back to JSON

---

## 9. Field Constraints

Use `Field` for extra validation:

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(min_length=1, max_length=50)
    age: int = Field(gt=0, le=150)
```

| Constraint | Meaning |
|------------|---------|
| `min_length` / `max_length` | String length limits |
| `gt` | Greater than |
| `ge` | Greater than or equal |
| `lt` / `le` | Less than (or equal) |

---

## 10. Reserved Python Keywords

Do not use Python keywords as field names:

```python
# Wrong — pass is a keyword
class Bad(BaseModel):
    pass: str

# Correct
class Good(BaseModel):
    password: str
```

---

## Summary

* Pydantic models are classes that inherit from `BaseModel`.
* Type hints on fields define the expected data shape.
* Pydantic validates data automatically and produces clear errors.
* Use separate input/output models to hide sensitive fields.
* FastAPI uses Pydantic models for request bodies, response filtering, and `/docs` generation.

For database-backed APIs, see [15 – Databases](15-databases.md) and [SQLModel with FastAPI](../libraries/fastapi/17-sqlmodel.md).
