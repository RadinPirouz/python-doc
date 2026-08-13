# FastAPI – POST Requests with JSON Body (`Body`)

## Prerequisites

* [33 – JSON](../../fundamentals/33-json.md) — JSON request bodies
* [36 – Pydantic basics](../../fundamentals/36-pydantic-basics.md) — prefer models over raw `Body()` for complex data
* [11-post-types](11-post-types.md) — Content-Type overview

This document demonstrates how to receive data from the **request body** using `Body()` in FastAPI.
This approach is commonly used when clients send **JSON payloads**.

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, Body, status
from fastapi.responses import JSONResponse

app = FastAPI()

users_db = [
    {"id": "1", "name": "radin", "password": "123"}
]


@app.post("/user/")
def get_user_from_body(
    target: int = Body(
        ...,
        embed=True,
        gt=0,
        alias="User ID",
        description="Enter target unique ID",
    )
):
    for item in users_db:
        if item["id"] == str(target):
            return JSONResponse(
                content={"msg": f"Your target user name is {item['name']}"},
                status_code=status.HTTP_200_OK,
            )

    return JSONResponse(
        content={"msg": "user not found"},
        status_code=status.HTTP_404_NOT_FOUND,
    )
```

---

## JSON Body Overview

### What Is a Request Body

The request body contains structured data sent by the client, most commonly as **JSON**.

**Content-Type:**

```
application/json
```

---

## Request Format

### Example JSON Payload

```json
{
  "User ID": 1
}
```

* `embed=True` requires the value to be wrapped inside a JSON object
* The key name is controlled by the `alias`

---

## Example Request Using `curl`

```bash
curl -X POST "http://localhost:8000/user/" \
  -H "Content-Type: application/json" \
  -d '{"User ID": 1}'
```

---

## Response Examples

### Success Response

```json
{
  "msg": "Your target user name is radin"
}
```

Status Code: **200 OK**

---

### Failure Response

```json
{
  "msg": "user not found"
}
```

Status Code: **404 Not Found**

---

## `Body()` Parameter Configuration

```python
Body(
    ...,
    embed=True,
    gt=0,
    alias="User ID",
    description="Enter target unique ID"
)
```

| Parameter     | Purpose                          |
| ------------- | -------------------------------- |
| `...`         | Field is required                |
| `embed=True`  | Wraps the value in a JSON object |
| `gt=0`        | Validates input value            |
| `alias`       | Custom JSON key name             |
| `description` | Shown in API documentation       |

---

## Swagger / OpenAPI Behavior

* JSON schema is automatically generated
* Validation errors are returned if rules are violated
* Aliases and descriptions appear in Swagger UI
* Request body is clearly documented

---

## Running the Application

Start the service using `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* Prefer request body over query parameters for POST requests
* Use Pydantic models instead of raw `Body()` for complex payloads
* Keep JSON structures consistent
* Avoid spaces in JSON keys for production APIs
* Never send sensitive data in plain text
* Use HTTPS for all JSON-based APIs
