# 12 – HTTP Basics

This document explains **HTTP** (Hypertext Transfer Protocol) — how web browsers, mobile apps, and other clients communicate with servers. Every FastAPI endpoint is an HTTP handler.

No prior networking knowledge is required. Read [11-json.md](11-json.md) next if you want to understand the data format APIs use.

---

## 1. Client and Server

```
Client  ──HTTP request──▶  Server
Client  ◀──HTTP response──  Server
```

| Role | Examples |
|------|----------|
| **Client** | Browser, mobile app, `curl`, Python `requests` |
| **Server** | FastAPI application running behind Uvicorn |

The client sends a **request**. The server processes it and sends back a **response**.

---

## 2. Parts of a URL

```
http://localhost:8000/users/42?active=true&limit=10
└─┬─┘ └────┬────┘└┬┘└─┬──┘ └──────────┬──────────┘
scheme    host   port path      query string
```

| Part | Example | Purpose |
|------|---------|---------|
| Scheme | `http`, `https` | Protocol (`https` is encrypted) |
| Host | `localhost`, `api.example.com` | Server address |
| Port | `8000`, `443` | Network port (often hidden for 80/443) |
| Path | `/users/42` | Resource location on the server |
| Query string | `?active=true&limit=10` | Optional filters and options |

---

## 3. HTTP Methods

Methods describe **what the client wants to do**:

| Method | Purpose | Safe to repeat? | Example |
|--------|---------|-----------------|---------|
| **GET** | Read data | Yes | List users |
| **POST** | Create data | No | Register a user |
| **PUT** | Replace/update resource | Yes* | Update user profile |
| **PATCH** | Partial update | Yes* | Change one field |
| **DELETE** | Remove resource | Yes* | Delete a user |

*Idempotent means calling the same request multiple times has the same effect as calling it once.

FastAPI maps methods to functions with decorators:

```python
@app.get("/users")
@app.post("/users")
@app.put("/users/{id}")
@app.delete("/users/{id}")
```

See [08-decorators.md](08-decorators.md) if decorators are new to you.

---

## 4. Path Parameters vs Query Parameters

### Path parameters

Embedded in the URL path. Usually identify a **specific resource**.

```
GET /users/42
         └── path parameter: id = 42
```

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

### Query parameters

Appended after `?`. Usually used for **filtering, sorting, pagination**.

```
GET /users?active=true&limit=10
           └──────── query parameters ────────┘
```

```python
@app.get("/users")
def list_users(active: bool = True, limit: int = 10):
    return {"active": active, "limit": limit}
```

| Feature | Path parameter | Query parameter |
|---------|----------------|-----------------|
| Location | In the URL path | After `?` |
| Required by default | Yes | No (if default provided) |
| Typical use | Resource ID | Filters, options |

---

## 5. Request Headers

Headers are metadata key-value pairs sent with every request:

```http
GET /users HTTP/1.1
Host: localhost:8000
Accept: application/json
Authorization: Bearer eyJhbGciOi...
Content-Type: application/json
```

| Header | Purpose |
|--------|---------|
| `Content-Type` | Format of the request body |
| `Accept` | Format the client wants in the response |
| `Authorization` | Credentials (tokens, API keys) |

---

## 6. Request Body

The body carries data for `POST`, `PUT`, and `PATCH` requests. `GET` and `DELETE` usually have no body.

```http
POST /users HTTP/1.1
Content-Type: application/json

{"name": "ali", "age": 25}
```

For form submissions:

```http
POST /login HTTP/1.1
Content-Type: application/x-www-form-urlencoded

username=admin&password=secret
```

For file uploads:

```http
POST /upload HTTP/1.1
Content-Type: multipart/form-data
```

---

## 7. HTTP Status Codes

Status codes tell the client whether the request succeeded and what happened.

### Common codes

| Code | Name | When to use |
|------|------|-------------|
| **200** | OK | Successful GET, PUT, PATCH |
| **201** | Created | Resource created (POST) |
| **204** | No Content | Successful DELETE (no body) |
| **400** | Bad Request | Invalid input from client |
| **401** | Unauthorized | Missing or invalid authentication |
| **403** | Forbidden | Authenticated but not allowed |
| **404** | Not Found | Resource does not exist |
| **422** | Unprocessable Entity | Validation failed (common in FastAPI) |
| **500** | Internal Server Error | Unexpected server error |

### Response structure

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{"detail": "User not found"}
```

The status code is in the first line. The body (often JSON) explains the error.

---

## 8. REST API Conventions

REST is a style of designing APIs with HTTP:

| Action | Method | Path example |
|--------|--------|--------------|
| List items | GET | `/users` |
| Get one item | GET | `/users/{id}` |
| Create item | POST | `/users` |
| Replace item | PUT | `/users/{id}` |
| Delete item | DELETE | `/users/{id}` |

Use plural nouns for collections (`/users`, not `/user`). Use path parameters for IDs (`/users/42`).

---

## 9. Testing with curl

`curl` is a command-line HTTP client useful for testing APIs:

```bash
# GET
curl http://localhost:8000/users

# GET with query parameter
curl "http://localhost:8000/users?active=true"

# POST with JSON body
curl -X POST "http://localhost:8000/users" \
  -H "Content-Type: application/json" \
  -d '{"name": "ali", "age": 25}'

# DELETE
curl -X DELETE "http://localhost:8000/users/42"
```

FastAPI also provides interactive docs at `/docs` (Swagger UI).

---

## 10. Client vs Server Perspective

| Concept | Client sends | Server returns |
|---------|--------------|----------------|
| Request method | GET, POST, etc. | — |
| Status code | — | 200, 404, etc. |
| Body | JSON, form data, files | JSON, HTML, etc. |
| Headers | Content-Type, Auth | Content-Type |

When you write FastAPI code, you are building the **server** side. Tools like `requests` or `curl` act as **clients**.

See [requests library overview](../libraries/requests/01-overview.md) for the client perspective in Python.

---

## Summary

* HTTP is the protocol clients and servers use to exchange messages.
* URLs have a path (resource location) and optional query string (filters).
* Methods (GET, POST, PUT, DELETE) express the intended action.
* Status codes communicate success or failure.
* Request and response bodies often contain JSON for APIs.
* FastAPI endpoints are functions that handle HTTP requests and return HTTP responses.
