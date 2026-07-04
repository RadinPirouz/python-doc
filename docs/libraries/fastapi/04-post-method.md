# FastAPI – POST Endpoint and JSON Input

## Overview

This document explains how to create a `POST` endpoint in FastAPI for adding new users. It covers two approaches:

1. Sending input as query parameters
2. Sending input as a JSON request body using a Pydantic model

For real API development, the Pydantic model approach is recommended because it provides a cleaner structure, better validation, and more realistic request handling.

---

# 1. Example Application Using Query Parameters

Create or update `main.py` with the following code:

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


@app.post("/new_user")
def create_user(name: str, age: int):
    new_user = {"name": name, "age": age}
    users.append(new_user)
    return {"msg": "User created successfully"}
```

## Explanation

```python
@app.post("/new_user")
def create_user(name: str, age: int):
```

This registers a `POST` endpoint at:

```http
POST /new_user
```

The endpoint receives two parameters:

```python
name: str
age: int
```

FastAPI automatically validates the input types. If `age` is not an integer, FastAPI returns a validation error.

## Example Request

```bash
curl -X POST "http://localhost:8000/new_user?name=ali&age=25"
```

## Example Response

```json
{
  "msg": "User created successfully"
}
```

## Important Note

In this version, the data is sent through query parameters, not as a JSON body.

This is acceptable for simple testing, but it is not the best practice for real APIs that create resources.

---

# 2. Recommended Application Using JSON Body

A better approach is to define a Pydantic model and receive the user data as JSON.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class User(BaseModel):
    name: str
    age: int


users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]


@app.get("/")
def home_page():
    return {"msg": "API is working"}


@app.post("/new_user")
def create_user(user: User):
    new_user = {"name": user.name, "age": user.age}
    users.append(new_user)
    return {"msg": "User created successfully"}
```

## Explanation

```python
class User(BaseModel):
    name: str
    age: int
```

This creates a request model named `User`.

The model defines the expected structure of the JSON input:

```json
{
  "name": "ali",
  "age": 25
}
```

FastAPI uses this model to:

Validate incoming data
Convert JSON into a Python object
Generate automatic API documentation
Return useful validation errors when input is invalid

---

# 3. POST Endpoint Definition

```python
@app.post("/new_user")
def create_user(user: User):
```

This creates a `POST` endpoint at:

```http
POST /new_user
```

The endpoint expects a JSON request body matching the `User` model.

---

# 4. Creating a New Resource

```python
new_user = {"name": user.name, "age": user.age}
users.append(new_user)
```

This code creates a new dictionary using the received request data and appends it to the `users` list.

The `users` list acts as temporary in-memory storage.

This simulates inserting a new record into a database.

---

# 5. Returning a JSON Response

```python
return {"msg": "User created successfully"}
```

FastAPI automatically converts the returned dictionary into a JSON response.

Example response:

```json
{
  "msg": "User created successfully"
}
```

---

# 6. Example Request Using curl

```bash
curl -X POST "http://localhost:8000/new_user" \
  -H "Content-Type: application/json" \
  -d '{"name": "ali", "age": 25}'
```

## Response

```json
{
  "msg": "User created successfully"
}
```

---

# 7. Verifying the Result

To verify that the user was added, you should also define a `GET /users` endpoint.

```python
@app.get("/users")
def get_users():
    return users
```

Then send this request:

```http
GET /users
```

Example response:

```json
[
  {
    "name": "abbas",
    "age": 20
  },
  {
    "name": "mmd",
    "age": 37
  },
  {
    "name": "asghar",
    "age": 19
  },
  {
    "name": "ali",
    "age": 25
  }
]
```

---

# 8. Complete Recommended Version

```python
from fastapi import FastAPI, status
from pydantic import BaseModel

app = FastAPI()


class User(BaseModel):
    name: str
    age: int


users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]


@app.get("/")
def home_page():
    return {"msg": "API is working"}


@app.get("/users")
def get_users():
    return users


@app.post("/new_user", status_code=status.HTTP_201_CREATED)
def create_user(user: User):
    new_user = {"name": user.name, "age": user.age}
    users.append(new_user)

    return {
        "msg": "User created successfully",
        "user": new_user
    }
```

---

# 9. Running the Application

Start the FastAPI application using `uvicorn`:

```bash
uvicorn main:app --reload
```

The API will run at:

```text
http://localhost:8000
```

Interactive API documentation will be available at:

```text
http://localhost:8000/docs
```

---

# 10. Best Practices

Use `POST` requests when creating new resources.

Use Pydantic models for request bodies instead of query parameters.

Return proper HTTP status codes, such as:

```http
201 Created
```

Do not use in-memory lists for production data storage.

Use a real database such as PostgreSQL, MySQL, or MongoDB in production.

Validate and sanitize all client-provided input.

Keep model names clear and professional. For example, use `User` instead of `user_class`.

Return meaningful responses that include the created resource when useful.

Use `/docs` to test and verify API behavior during development.

---

# 11. Production Note for DevOps

The in-memory `users` list is reset every time the application restarts. In production environments, application data must be stored in a persistent database.

For production deployment, the FastAPI application should usually run behind a process manager and reverse proxy, such as:

```text
Gunicorn / Uvicorn workers
Nginx or Traefik
Docker or Kubernetes
PostgreSQL or another persistent database
```

The development command:

```bash
uvicorn main:app --reload
```

should not be used in production because `--reload` is intended only for local development.
