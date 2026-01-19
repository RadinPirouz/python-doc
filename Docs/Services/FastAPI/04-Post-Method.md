# FastAPI – POST Endpoint and JSON Input

This section demonstrates how to handle **POST requests** in FastAPI to create new resources using request data and return JSON responses.

---

## Example Application (POST Request)

Extend `main.py` with the following code:

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

---

## Code Overview

### POST Endpoint Definition

```python
@app.post("/new_user")
def create_user(name: str, age: int):
```

* Registers an HTTP **POST** endpoint at `/new_user`
* Accepts input parameters:

  * `name` → string
  * `age` → integer
* FastAPI automatically validates input types

---

### Creating a New Resource

```python
new_user = {"name": name, "age": age}
users.append(new_user)
```

* Constructs a new user object
* Appends it to the in-memory `users` list
* Simulates creating a record in a database

---

### JSON Response

```python
return {"msg": "User created successfully"}
```

* Returns a structured JSON response
* Automatically serialized by FastAPI

---

## Example Request

### Using `curl`

```bash
curl -X POST "http://localhost:8000/new_user?name=ali&age=25"
```

### Response

```json
{
  "msg": "User created successfully"
}
```

---

## Verifying the Result

After creating a user, retrieve the updated list:

```http
GET /users
```

Response will now include the newly added user.

---

## Running the Application

Start the FastAPI service using `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* POST requests should be used to create resources
* Avoid modifying in-memory data in production environments
* Use request bodies with Pydantic models instead of query parameters for real APIs
* Return appropriate HTTP status codes (`201 Created`)
* Validate and sanitize all client-provided input
* Replace in-memory storage with persistent databases

