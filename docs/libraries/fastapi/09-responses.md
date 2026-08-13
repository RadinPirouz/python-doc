# FastAPI – Response Model and JSONResponse

## Prerequisites

* [36 – Pydantic basics](../../fundamentals/36-pydantic-basics.md) — `BaseModel`, input vs output models
* [33 – JSON](../../fundamentals/33-json.md) — JSON responses
* [34 – HTTP basics](../../fundamentals/34-http-basics.md) — status codes
* [19 – Classes](../../fundamentals/19-class.md) — classes (if `BaseModel` is unfamiliar)

## Overview

This document explains two important FastAPI response concepts:

1. Using `response_model` to control what data is returned to the client
2. Using `JSONResponse` when you need explicit control over the response body, status code, or headers

Response models are especially useful when you want to hide sensitive fields such as passwords from API responses.

---

# 1. Response Model

## Incorrect Example

The following code has several issues:

```python
from fastapi import FastAPI, status , HTTPExption
from pydantic import BaseModel

app = FastAPI()


class usersin(BaseModel):
    username: str
    pass: str 

class usersout(BaseModel):
    username: str


@app.post("/user")
def home(user: usersin, responce_model=usersout):
    return user
```

## Problems in the Code

### 1. `HTTPExption` is misspelled

Correct import:

```python
HTTPException
```

### 2. `pass` cannot be used as a field name

`pass` is a reserved keyword in Python.

Instead of:

```python
pass: str
```

Use:

```python
password: str
```

### 3. `response_model` is written in the wrong place

This is incorrect:

```python
def home(user: usersin, responce_model=usersout):
```

`response_model` must be passed inside the route decorator.

Correct:

```python
@app.post("/user", response_model=UserOut)
```

### 4. `responce_model` is misspelled

Correct spelling:

```python
response_model
```

---

# 2. Correct Response Model Example

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class UserIn(BaseModel):
    username: str
    password: str


class UserOut(BaseModel):
    username: str


@app.post("/user", response_model=UserOut)
def create_user(user: UserIn):
    return user
```

---

# 3. How Response Model Works

## Input Model

```python
class UserIn(BaseModel):
    username: str
    password: str
```

This model defines the data that the API receives from the client.

Example request body:

```json
{
  "username": "abbas",
  "password": "123456"
}
```

The API accepts both fields:

```text
username
password
```

---

## Output Model

```python
class UserOut(BaseModel):
    username: str
```

This model defines the data that the API returns to the client.

Even though the endpoint receives the password, the response only returns the username.

Example response:

```json
{
  "username": "abbas"
}
```

The password is removed from the response automatically.

---

# 4. Endpoint Definition

```python
@app.post("/user", response_model=UserOut)
def create_user(user: UserIn):
    return user
```

This creates a `POST` endpoint at:

```http
POST /user
```

The endpoint receives data based on `UserIn` and returns data based on `UserOut`.

FastAPI uses `response_model` to filter the response before sending it to the client.

---

# 5. Example Request

## Using curl

```bash
curl -X POST "http://localhost:8000/user" \
  -H "Content-Type: application/json" \
  -d '{"username": "abbas", "password": "123456"}'
```

## Response

```json
{
  "username": "abbas"
}
```

The password is not included in the response.

---

# 6. Why Use Response Models

Response models are useful because they help you:

Protect sensitive data
Keep API responses consistent
Control exactly what the client receives
Improve automatic documentation
Validate response data before sending it
Separate input schemas from output schemas

---

# 7. Better Version with Status Code

For user creation endpoints, it is better to return `201 Created`.

```python
from fastapi import FastAPI, status
from pydantic import BaseModel

app = FastAPI()


class UserIn(BaseModel):
    username: str
    password: str


class UserOut(BaseModel):
    username: str


@app.post(
    "/user",
    response_model=UserOut,
    status_code=status.HTTP_201_CREATED
)
def create_user(user: UserIn):
    return user
```

---

# 8. JSONResponse

FastAPI automatically converts Python dictionaries into JSON responses.

For most endpoints, this is enough:

```python
@app.get("/")
def home():
    return {"msg": "API is working"}
```

However, FastAPI also allows you to use `JSONResponse` when you need more control.

---

# 9. Example Application Using JSONResponse

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, status
from fastapi.responses import JSONResponse

app = FastAPI()


@app.get("/")
def home():
    return JSONResponse(
        content={"msg": "API is working"},
        status_code=status.HTTP_200_OK
    )
```

---

# 10. Response Behavior

## Endpoint

```http
GET /
```

## Response Body

```json
{
  "msg": "API is working"
}
```

## HTTP Status Code

```http
200 OK
```

---

# 11. Default Response vs JSONResponse

## Default FastAPI Response

```python
@app.get("/")
def home():
    return {"msg": "API is working"}
```

This is the recommended style for most simple APIs.

FastAPI automatically serializes the dictionary into JSON.

---

## Explicit JSONResponse

```python
return JSONResponse(
    content={"msg": "API is working"},
    status_code=status.HTTP_200_OK
)
```

This gives more direct control over the response.

---

# 12. When to Use JSONResponse

Use `JSONResponse` when you need to:

Return dynamic status codes
Add custom headers
Customize the response structure manually
Return responses from exception handlers
Return responses from middleware
Override FastAPI’s default response behavior

---

# 13. Example: JSONResponse with Custom Status Code

```python
from fastapi import FastAPI, status
from fastapi.responses import JSONResponse

app = FastAPI()


@app.post("/login")
def login():
    return JSONResponse(
        content={"msg": "Login successful"},
        status_code=status.HTTP_200_OK
    )
```

---

# 14. Example: JSONResponse with Headers

```python
from fastapi import FastAPI
from fastapi.responses import JSONResponse

app = FastAPI()


@app.get("/custom")
def custom_response():
    return JSONResponse(
        content={"msg": "Custom response"},
        headers={"X-App-Version": "1.0.0"}
    )
```

---

# 15. Complete Example with Response Model and JSONResponse

```python
from fastapi import FastAPI, status
from fastapi.responses import JSONResponse
from pydantic import BaseModel

app = FastAPI()


class UserIn(BaseModel):
    username: str
    password: str


class UserOut(BaseModel):
    username: str


@app.get("/")
def home():
    return JSONResponse(
        content={"msg": "API is working"},
        status_code=status.HTTP_200_OK
    )


@app.post(
    "/user",
    response_model=UserOut,
    status_code=status.HTTP_201_CREATED
)
def create_user(user: UserIn):
    return user
```

---

# 16. Running the Application

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

# 17. Best Practices

Use `response_model` to control API output.

Never return sensitive data such as passwords, tokens, or secrets.

Use separate models for input and output.

Use clear class names such as `UserIn` and `UserOut`.

Use `password` instead of `pass` because `pass` is a reserved Python keyword.

Place `response_model` inside the route decorator, not inside the function parameters.

Prefer returning normal dictionaries for simple responses.

Use `JSONResponse` only when extra control is required.

Use proper HTTP status codes, such as:

```http
200 OK
201 Created
400 Bad Request
401 Unauthorized
404 Not Found
500 Internal Server Error
```

Do not use `uvicorn --reload` in production.

---

# 18. DevOps Production Note

In production, the FastAPI application should usually run behind a production-grade ASGI server setup and a reverse proxy.

A common production stack is:

```text
FastAPI
Gunicorn with Uvicorn workers
Nginx or Traefik
Docker or Kubernetes
PostgreSQL or another persistent database
```

The development command:

```bash
uvicorn main:app --reload
```

is only for local development.

For production, use a more stable process configuration, such as Gunicorn with Uvicorn workers, container health checks, logging, monitoring, and proper secret management.
