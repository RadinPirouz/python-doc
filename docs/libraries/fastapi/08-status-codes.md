# FastAPI – HTTP Status Codes and Error Handling

## Prerequisites

* [12 – HTTP basics](../../fundamentals/12-http-basics.md) — status codes (200, 404, etc.)
* [07 – Error handling](../../fundamentals/07-error-handling.md) — exceptions and raising errors

This document demonstrates how to use **HTTP status codes** in FastAPI to accurately represent the result of an API operation.
Correct status codes improve API reliability, observability, and client-side behavior.

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, status, HTTPException

app = FastAPI()

users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]


@app.get("/", status_code=status.HTTP_200_OK)
def home_page():
    return {"msg": "API is working"}


@app.get("/users", status_code=status.HTTP_200_OK)
def show_users():
    return users


@app.post("/create_user", status_code=status.HTTP_201_CREATED)
def create_user(name: str, age: int):
    new_user = {"name": name, "age": age}
    users.append(new_user)
    return {"msg": f"user {name} with age {age} created"}


@app.put("/update_user/{target_name}", status_code=status.HTTP_202_ACCEPTED)
def update_user(target_name: str, age: int):
    for user in users:
        if user["name"] == target_name:
            user["age"] = age
            return {"msg": f"user {target_name} updated"}
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="User not found"
    )


@app.delete("/delete_user/{target_name}", status_code=status.HTTP_204_NO_CONTENT)
def delete_user(target_name: str):
    for user in users:
        if user["name"] == target_name:
            users.remove(user)
            return
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="User not found"
    )
```

---

## Status Code Overview

HTTP status codes communicate the result of an API request. See [12 – HTTP basics](../../fundamentals/12-http-basics.md#7-http-status-codes) for a full explanation.

| Code | Meaning    | Usage                             |
| ---- | ---------- | --------------------------------- |
| 200  | OK         | Successful GET requests           |
| 201  | Created    | Resource successfully created     |
| 202  | Accepted   | Update request accepted           |
| 204  | No Content | Resource deleted successfully     |
| 404  | Not Found  | Requested resource does not exist |

---

## Endpoint Behavior

### Root Endpoint

```http
GET /
```

* Returns HTTP **200 OK**
* Used as a health check

---

### Create User

```http
POST /create_user
```

* Returns HTTP **201 Created**
* Indicates successful resource creation

---

### Update User

```http
PUT /update_user/{target_name}
```

* Returns HTTP **202 Accepted** on success
* Raises HTTP **404 Not Found** if user does not exist

---

### Delete User

```http
DELETE /delete_user/{target_name}
```

* Returns HTTP **204 No Content** on success
* Returns no response body
* Raises HTTP **404 Not Found** if user does not exist

---

## Error Handling with `HTTPException`

```python
raise HTTPException(
    status_code=status.HTTP_404_NOT_FOUND,
    detail="User not found"
)
```

* Immediately stops request processing
* Returns structured error responses
* Automatically serialized by FastAPI

**Error Response Example:**

```json
{
  "detail": "User not found"
}
```

---

## Example Requests

### Create User

```bash
curl -X POST "http://localhost:8000/create_user?name=ali&age=25"
```

### Update User

```bash
curl -X PUT "http://localhost:8000/update_user/ali?age=30"
```

### Delete User

```bash
curl -X DELETE "http://localhost:8000/delete_user/ali"
```

---

## Running the Application

Start the application using `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* Always return meaningful HTTP status codes
* Use `status` module instead of hard-coded numbers
* Use `HTTPException` for predictable error handling
* Do not return response bodies with `204 No Content`
* Align status codes with REST conventions
* Ensure monitoring systems rely on status codes, not messages
* Standardize error formats across services

