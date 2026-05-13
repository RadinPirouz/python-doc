# FastAPI – GET Endpoints and JSON Responses

## Overview

This document explains how to create multiple `GET` endpoints in FastAPI, return JSON responses, and use path parameters to retrieve specific data from an in-memory dataset.

The example uses a simple list of users to simulate a database.

---

# 1. Example Application

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
def root_dir():
    return {"message": "API is working"}


@app.get("/users")
def get_users():
    return users


@app.get("/user/{name_input}")
def get_user_by_name(name_input: str):
    for item in users:
        if item["name"] == name_input:
            return {"information": item}

    return {"message": "User not found"}
```

---

# 2. Application Initialization

```python
app = FastAPI()
```

This creates the main FastAPI application instance.

The variable `app` is used by `uvicorn` when starting the service.

Example:

```bash
uvicorn main:app --reload
```

In `main:app`:

```text
main = Python file name
app = FastAPI application instance
```

---

# 3. In-Memory Data Store

```python
users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]
```

This list is used as temporary storage.

Each user is represented as a Python dictionary.

FastAPI can automatically convert these dictionaries into JSON responses.

Example Python object:

```python
{"name": "abbas", "age": 20}
```

Example JSON response:

```json
{
  "name": "abbas",
  "age": 20
}
```

## Important Note

This in-memory list is only suitable for learning, development, and testing.

If the application restarts, the data will be reset.

For production, use a persistent database such as PostgreSQL, MySQL, MongoDB, or another database system.

---

# 4. Defined Endpoints

The application defines three `GET` endpoints:

```http
GET /
GET /users
GET /user/{name_input}
```

---

# 5. Root Endpoint

```python
@app.get("/")
def root_dir():
    return {"message": "API is working"}
```

This endpoint is available at:

```http
GET /
```

It returns a simple JSON response:

```json
{
  "message": "API is working"
}
```

This type of endpoint is commonly used as a simple health check.

For example, load balancers, monitoring systems, or Kubernetes probes can use it to check whether the API is reachable.

---

# 6. Get All Users Endpoint

```python
@app.get("/users")
def get_users():
    return users
```

This endpoint is available at:

```http
GET /users
```

It returns the full list of users.

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
  }
]
```

FastAPI automatically serializes the Python list into a JSON array.

---

# 7. Get User by Name Endpoint

```python
@app.get("/user/{name_input}")
def get_user_by_name(name_input: str):
    for item in users:
        if item["name"] == name_input:
            return {"information": item}

    return {"message": "User not found"}
```

This endpoint is available at:

```http
GET /user/{name_input}
```

The `{name_input}` part is a path parameter.

Example request:

```http
GET /user/abbas
```

In this request, FastAPI assigns:

```python
name_input = "abbas"
```

The function then searches the `users` list for a user whose name matches the input.

---

# 8. Successful Response Example

Request:

```http
GET /user/abbas
```

Response:

```json
{
  "information": {
    "name": "abbas",
    "age": 20
  }
}
```

---

# 9. Failure Response Example

Request:

```http
GET /user/ali
```

Response:

```json
{
  "message": "User not found"
}
```

In the current simple version, the API returns a normal JSON message even when the user does not exist.

However, in a real API, it is better to return a proper `404 Not Found` response.

---

# 10. Path Parameters

A path parameter is a dynamic part of the URL.

In this route:

```python
@app.get("/user/{name_input}")
```

`name_input` is a path parameter.

Example:

```http
GET /user/mmd
```

FastAPI extracts the value from the URL and passes it to the function:

```python
def get_user_by_name(name_input: str):
```

Because `name_input` is defined as a string:

```python
name_input: str
```

FastAPI validates it as a string.

---

# 11. Better Version with HTTPException

The previous version works, but it does not return the correct HTTP status code when a user is not found.

A better version uses `HTTPException`.

```python
from fastapi import FastAPI, HTTPException, status

app = FastAPI()

users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]


@app.get("/")
def root_dir():
    return {"message": "API is working"}


@app.get("/users")
def get_users():
    return users


@app.get("/user/{name_input}")
def get_user_by_name(name_input: str):
    for item in users:
        if item["name"] == name_input:
            return {"information": item}

    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="User not found"
    )
```

---

# 12. Improved Failure Response

Request:

```http
GET /user/ali
```

Response:

```json
{
  "detail": "User not found"
}
```

HTTP status code:

```http
404 Not Found
```

This is better because the API response now correctly tells the client that the requested resource does not exist.

---

# 13. Running the Application

Start the FastAPI application using `uvicorn`:

```bash
uvicorn main:app --reload
```

The application will be available at:

```text
http://localhost:8000
```

---

# 14. Accessing the Endpoints

Root endpoint:

```text
http://localhost:8000/
```

Get all users:

```text
http://localhost:8000/users
```

Get user by name:

```text
http://localhost:8000/user/abbas
```

Interactive API documentation:

```text
http://localhost:8000/docs
```

Alternative API documentation:

```text
http://localhost:8000/redoc
```

---

# 15. Testing with curl

## Test Root Endpoint

```bash
curl http://localhost:8000/
```

Response:

```json
{
  "message": "API is working"
}
```

## Test Get All Users

```bash
curl http://localhost:8000/users
```

Response:

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
  }
]
```

## Test Get User by Name

```bash
curl http://localhost:8000/user/abbas
```

Response:

```json
{
  "information": {
    "name": "abbas",
    "age": 20
  }
}
```

## Test User Not Found

```bash
curl http://localhost:8000/user/ali
```

Response:

```json
{
  "detail": "User not found"
}
```

---

# 16. Complete Recommended Version

```python
from fastapi import FastAPI, HTTPException, status

app = FastAPI()

users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]


@app.get("/")
def root_dir():
    return {"message": "API is working"}


@app.get("/users")
def get_users():
    return users


@app.get("/user/{name_input}")
def get_user_by_name(name_input: str):
    for item in users:
        if item["name"] == name_input:
            return {"information": item}

    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="User not found"
    )
```

---

# 17. Best Practices

Use `GET` endpoints only for retrieving data.

Do not use `GET` requests to create, update, or delete resources.

Return structured JSON responses.

Use proper HTTP status codes.

Use `404 Not Found` when a requested resource does not exist.

Use `200 OK` when the request is successful.

Replace in-memory lists with a real database in production.

Keep route names clear and consistent.

Use plural naming for collection endpoints, such as:

```http
GET /users
```

Use specific resource endpoints for single items, such as:

```http
GET /users/{username}
```

As the project grows, separate code into different files for routes, models, services, and database logic.
