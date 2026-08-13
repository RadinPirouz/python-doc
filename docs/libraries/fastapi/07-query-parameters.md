# FastAPI – Query Parameters

## Prerequisites

* [34 – HTTP basics](../../fundamentals/34-http-basics.md) — query strings in URLs
* [32 – Type hints](../../fundamentals/32-type-hints.md) — `Optional`, `Annotated`, `str | None`

This document demonstrates how to use **query parameters** in FastAPI using multiple typing styles, including:

* Native Python union types (`str | None`)
* `Optional` from `typing`
* `Annotated` with `Query` validation

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, Query
from typing import Optional, Annotated

app = FastAPI()

users = [
    {"name": "abbas", "age": 20},
    {"name": "abbas", "age": 32},
    {"name": "abbas", "age": 54},
    {"name": "abbas", "age": 15},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]


@app.get("/")
def home_page():
    return {"msg": "API is working"}


@app.get("/user")
def get_users(target_name: str | None = None):
    if target_name:
        return [item for item in users if item["name"] == target_name]
    return users


@app.get("/user2")
def get_users_optional(target_name: Optional[str] = None):
    if target_name:
        return [item for item in users if item["name"] == target_name]
    return users


@app.get("/user3")
def get_users_annotated(
    target_name: Annotated[str | None, Query(max_length=50)] = None
):
    if target_name:
        return [item for item in users if item["name"] == target_name]
    return users
```

---

## Query Parameter Overview

Query parameters are key-value pairs appended to the URL after `?`. See [34 – HTTP basics](../../fundamentals/34-http-basics.md#4-path-parameters-vs-query-parameters).

**Example:**

```
/user?target_name=abbas
```

---

## Defined Endpoints

### 1. Basic Query Parameter (Python Union Type)

```python
@app.get("/user")
def get_users(target_name: str | None = None):
```

* Uses Python 3.10+ union syntax
* `target_name` is optional
* If not provided, all users are returned

**Example Request:**

```
GET /user?target_name=abbas
```

---

### 2. Optional Query Parameter (`typing.Optional`)

```python
@app.get("/user2")
def get_users_optional(target_name: Optional[str] = None):
```

* Uses `Optional[str]` for compatibility with older Python versions
* Behavior is identical to the first endpoint

**Example Request:**

```
GET /user2?target_name=abbas
```

---

### 3. Validated Query Parameter (`Annotated + Query`)

```python
@app.get("/user3")
def get_users_annotated(
    target_name: Annotated[str | None, Query(max_length=50)] = None
):
```

* Uses `Annotated` for metadata binding
* Adds validation rules:

  * `max_length=50`
* Automatically returns validation errors if constraints are violated

**Example Invalid Request:**

```
GET /user3?target_name=verylongnamethatexceedslimit...
```

**Response:**

```json
{
  "detail": [
    {
      "type": "string_too_long",
      "loc": ["query", "target_name"],
      "msg": "String should have at most 50 characters",
      "input": "..."
    }
  ]
}
```

---

## Example Responses

### Filtered Response

```json
[
  {"name": "abbas", "age": 20},
  {"name": "abbas", "age": 32},
  {"name": "abbas", "age": 54},
  {"name": "abbas", "age": 15}
]
```

---

### Default Response (No Query Parameter)

```json
[
  {"name": "abbas", "age": 20},
  {"name": "abbas", "age": 32},
  {"name": "abbas", "age": 54},
  {"name": "abbas", "age": 15},
  {"name": "mmd", "age": 37},
  {"name": "asghar", "age": 19}
]
```

---

## Running the Application

Start the service using `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* Use query parameters for filtering and searching
* Always provide defaults for optional parameters
* Use `Query()` for validation and constraints
* Return full datasets when no filters are applied
* Avoid large in-memory datasets in production
* Use pagination for large result sets
* Combine query parameters with database queries in real systems

