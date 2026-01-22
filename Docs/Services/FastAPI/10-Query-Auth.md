# FastAPI – Advanced Query Parameters (`Query`)

This document demonstrates how to configure **query parameters** in FastAPI using `Query()` to add:

* Aliases
* Descriptions (for API documentation)
* Deprecation warnings
* Validation and metadata

It also shows how these configurations appear in the **OpenAPI / Swagger UI**.

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, Query, status
from fastapi.responses import JSONResponse

app = FastAPI()

users_db = [
    {"id": "1", "name": "radin"}
]


@app.get("/user/")
def get_user_deprecated(
    target: int | None = Query(
        default=None,
        alias="User ID",
        description="Enter target unique ID",
        deprecated=True,
    )
):
    for item in users_db:
        if int(item["id"]) == target:
            return JSONResponse(
                content={"msg": f"Your target user name is {item['name']}"},
                status_code=status.HTTP_200_OK,
            )

    return JSONResponse(
        content={"msg": "user not found"},
        status_code=status.HTTP_404_NOT_FOUND,
    )


@app.get("/user")
def get_user(
    target: int | None = Query(
        default=None,
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

## Query Parameter Configuration

### `Query()` Parameters Used

```python
Query(
    default=None,
    alias="User ID",
    description="Enter target unique ID",
    deprecated=True
)
```

| Parameter     | Purpose                                     |
| ------------- | ------------------------------------------- |
| `default`     | Makes the query parameter optional          |
| `alias`       | Changes the query parameter name in the URL |
| `description` | Appears in Swagger UI documentation         |
| `deprecated`  | Marks the parameter as deprecated           |

---

## Example Requests

### Using Alias in Query String

```http
GET /user?User%20ID=1
```

---

## Endpoint Behavior

### Deprecated Endpoint

```http
GET /user/
```

* Query parameter is marked as **deprecated**
* Swagger UI displays a warning
* Useful for backward compatibility

---

### Active Endpoint

```http
GET /user
```

* Uses the same alias and description
* Recommended for active usage

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

## Swagger / OpenAPI Integration

* Alias appears as the query parameter name
* Description is visible in API docs
* Deprecated parameters are clearly marked
* Improves API discoverability and usability

---

## Running the Application

Start the service using `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* Use `Query()` for validation and documentation
* Avoid spaces in aliases for production APIs
* Deprecate parameters instead of removing them
* Keep backward compatibility during API evolution
* Prefer consistent parameter naming
* Use response models instead of manual `JSONResponse` when possible

