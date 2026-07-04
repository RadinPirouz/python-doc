# FastAPI – PUT Method 

## Prerequisites

* [12 – HTTP basics](../../fundamentals/12-http-basics.md) — PUT method, path and query parameters
* [10 – Type hints](../../fundamentals/10-type-hints.md) — parameter types
* Chapters [03-get-method](03-get-method.md) and [04-post-method](04-post-method.md) — prior CRUD examples

This document demonstrates how to use the **HTTP PUT method** in FastAPI to update an existing resource.
It builds on previous GET and POST examples and completes a basic CRUD-style workflow.

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI

app = FastAPI()

users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]


@app.get("/")
def home_page():
    return {"msg": "API is working"}


@app.get("/users")
def show_users():
    return users


@app.post("/create_user")
def create_user(name: str, age: int):
    new_user = {"name": name, "age": age}
    users.append(new_user)
    return {"msg": f"user {name} with age {age} created"}


@app.put("/update_user/{target_name}")
def update_user(target_name: str, age: int):
    for user in users:
        if user["name"] == target_name:
            user["age"] = age
            return {"msg": f"user {target_name} updated"}
    return {"msg": "user not found"}
```

---

## Code Overview

### PUT Endpoint Definition

```python
@app.put("/update_user/{target_name}")
def update_user(target_name: str, age: int):
```

* Registers an HTTP **PUT** endpoint
* `target_name` is a **path parameter**
* `age` is a **query parameter**
* Used to update an existing user’s data

---

### Update Logic

```python
for user in users:
    if user["name"] == target_name:
        user["age"] = age
```

* Iterates over the in-memory users list
* Matches user by name
* Updates the `age` field in place

---

### Success Response

```json
{
  "msg": "user abbas updated"
}
```

Returned when the target user exists and is updated successfully.

---

### Failure Response

```json
{
  "msg": "user not found"
}
```

Returned when no matching user exists.

---

## Example Requests

### Update User Age (PUT)

```bash
curl -X PUT "http://localhost:8000/update_user/abbas?age=25"
```

### Verify Update

```http
GET /users
```

Updated response:

```json
[
  {"name": "abbas", "age": 25},
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

## HTTP Method Summary

| Method | Endpoint              | Purpose                 |
| ------ | --------------------- | ----------------------- |
| GET    | `/`                   | Health check            |
| GET    | `/users`              | Retrieve all users      |
| POST   | `/create_user`        | Create a new user       |
| PUT    | `/update_user/{name}` | Update an existing user |

---

## Best Practices

* Use **PUT** for full updates and **PATCH** for partial updates
* Return proper HTTP status codes (`404`, `200`, `201`)
* Avoid using in-memory data stores in production
* Use Pydantic models for request bodies instead of query parameters
* Add input validation and error handling
* Separate routes, services, and models as the project grows
* Make PUT operations idempotent

