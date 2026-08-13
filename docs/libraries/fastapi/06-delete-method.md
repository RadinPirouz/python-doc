# FastAPI – DELETE Method (Remove Resource)

## Prerequisites

* [34 – HTTP basics](../../fundamentals/34-http-basics.md) — DELETE method
* Chapters [03](03-get-method.md)–[05](05-put-method.md) — prior CRUD examples

This document demonstrates how to use the **HTTP DELETE method** in FastAPI to remove an existing resource.
It completes the CRUD workflow using **GET, POST, PUT, and DELETE** operations.

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


@app.delete("/delete_user/{target_name}")
def delete_user(target_name: str):
    for user in users:
        if user["name"] == target_name:
            users.remove(user)
            return {"msg": f"user {target_name} deleted"}
    return {"msg": "user not found"}
```

---

## Code Overview

### DELETE Endpoint Definition

```python
@app.delete("/delete_user/{target_name}")
def delete_user(target_name: str):
```

* Registers an HTTP **DELETE** endpoint
* `target_name` is a **path parameter**
* Used to remove a user from the data store

---

### Delete Logic

```python
for user in users:
    if user["name"] == target_name:
        users.remove(user)
```

* Iterates through the users list
* Finds a matching user by name
* Removes the user from the list

---

### Success Response

```json
{
  "msg": "user abbas deleted"
}
```

Returned when the user is successfully removed.

---

### Failure Response

```json
{
  "msg": "user not found"
}
```

Returned when the specified user does not exist.

---

## Example Requests

### Delete a User

```bash
curl -X DELETE "http://localhost:8000/delete_user/abbas"
```

---

### Verify Deletion

```http
GET /users
```

Response:

```json
[
  {"name": "mmd", "age": 37},
  {"name": "asghar", "age": 19}
]
```

---

## Running the Application

Start the service with `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## CRUD Endpoint Summary

| Method | Endpoint              | Description        |
| ------ | --------------------- | ------------------ |
| GET    | `/`                   | Health check       |
| GET    | `/users`              | Retrieve all users |
| POST   | `/create_user`        | Create a user      |
| PUT    | `/update_user/{name}` | Update a user      |
| DELETE | `/delete_user/{name}` | Delete a user      |

---

## Best Practices

* Use **DELETE** only for resource removal
* Return appropriate HTTP status codes (`204`, `404`)
* Ensure delete operations are idempotent
* Avoid modifying in-memory data in production
* Add authentication and authorization for destructive operations
* Log delete actions for auditability
* Use database transactions when deleting persistent data

