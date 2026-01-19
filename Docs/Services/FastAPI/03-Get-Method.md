# FastAPI – GET Endpoints and JSON Responses

This document demonstrates how to define multiple **GET endpoints** in FastAPI, return JSON responses, and use **path parameters** to retrieve specific data from an in-memory dataset.

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

## Code Overview

### Application Initialization

```python
app = FastAPI()
```

Initializes the FastAPI application instance.

---

### In-Memory Data Store

```python
users = [
    {"name": "abbas", "age": 20},
    {"name": "mmd", "age": 37},
    {"name": "asghar", "age": 19},
]
```

* Simulates a database using a Python list
* Each user is represented as a JSON-compatible dictionary
* Suitable for development and testing purposes

---

## Defined Endpoints

### Root Endpoint

```http
GET /
```

**Response:**

```json
{
  "message": "API is working"
}
```

Used as a health check or readiness probe.

---

### Get All Users

```http
GET /users
```

**Response:**

```json
[
  {"name": "abbas", "age": 20},
  {"name": "mmd", "age": 37},
  {"name": "asghar", "age": 19}
]
```

Returns the full list of users as JSON.

---

### Get User by Name (Path Parameter)

```http
GET /user/{name_input}
```

**Example Request:**

```http
GET /user/abbas
```

**Successful Response:**

```json
{
  "information": {
    "name": "abbas",
    "age": 20
  }
}
```

**Failure Response:**

```json
{
  "message": "User not found"
}
```

---

## Path Parameters

* `name_input` is a dynamic path parameter
* Automatically validated and converted to `str` by FastAPI
* Used to filter data at runtime

---

## Running the Application

Use `uvicorn` to start the service:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* Use structured JSON responses (`key: value`) instead of tuples
* Validate user input when moving beyond in-memory data
* Replace in-memory storage with a database for production
* Use proper HTTP status codes (`404`, `200`) in real-world APIs
* Separate routing, models, and business logic as the project grows

