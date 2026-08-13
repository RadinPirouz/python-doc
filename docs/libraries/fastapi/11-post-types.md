# POST Request Types and Content Types

## Prerequisites

* [34 – HTTP basics](../../fundamentals/34-http-basics.md) — POST, request body, headers
* [33 – JSON](../../fundamentals/33-json.md) — JSON as the default API format

This document explains the **body types of POST requests** and the most common **Content-Types** used in APIs.
Understanding these formats is essential for building interoperable and production-ready FastAPI services.

---

## POST Request Body Types

A POST request sends data to the server in the **request body**.
The format of this data is defined by the **Content-Type** header.

---

## 1. Form Data

### Description

Used primarily for HTML form submissions. Common in browser-based applications.

### Content-Type

```
application/x-www-form-urlencoded
```

or

```
multipart/form-data
```

### Characteristics

* Key-value pairs
* Supports file uploads (multipart)
* Common in login and upload forms

### Example

```http
POST /login
Content-Type: application/x-www-form-urlencoded

username=admin&password=secret
```

---

## 2. JSON

### Description

The most common data format for REST APIs.

### Content-Type

```
application/json
```

### Characteristics

* Structured, readable, and language-independent
* Easily parsed and validated
* Default choice for FastAPI APIs

### Example

```json
{
  "name": "abbas",
  "age": 25
}
```

---

## 3. XML

### Description

An older but still widely used data format in enterprise systems and legacy APIs.

### Content-Type

```
application/xml
```

or

```
text/xml
```

### Characteristics

* Verbose and schema-driven
* Common in SOAP-based services
* Less common in modern REST APIs

### Example

```xml
<user>
  <name>abbas</name>
  <age>25</age>
</user>
```

---

## 4. Plain Text

### Description

Used when sending raw text without structure.

### Content-Type

```
text/plain
```

### Characteristics

* No schema or structure
* Useful for logs, messages, or simple payloads
* Requires custom parsing on the server

### Example

```
Hello FastAPI
```

---

## 5. Binary Data

### Description

Used for sending non-textual data such as images, videos, or files.

### Content-Type

```
application/octet-stream
```

### Characteristics

* Raw binary format
* Common for file uploads and downloads
* Requires stream handling

### Example

```
(binary file data)
```

---

## 6. GraphQL

### Description

GraphQL uses POST requests to execute queries and mutations.

### Content-Type

```
application/json
```

### Characteristics

* Single endpoint
* Flexible client-driven queries
* Requires GraphQL server support

### Example

```json
{
  "query": "query { users { id name } }"
}
```

---

## Common Content Types Summary

| Content-Type                        | Usage                      |
| ----------------------------------- | -------------------------- |
| `application/json`                  | REST APIs (default)        |
| `application/x-www-form-urlencoded` | HTML forms                 |
| `multipart/form-data`               | Forms with file uploads    |
| `application/xml`                   | Legacy / SOAP APIs         |
| `text/plain`                        | Raw text                   |
| `application/octet-stream`          | Binary data                |
| `application/graphql`               | GraphQL (rare, often JSON) |

---

## FastAPI Considerations

* JSON is the recommended default for FastAPI
* Use Pydantic models for JSON validation
* Use `Form()` and `File()` for form and file uploads
* Always validate content type in production APIs
* Document supported content types clearly

---

## Best Practices

* Choose the simplest format that meets requirements
* Prefer `application/json` for APIs
* Avoid XML unless required by integration
* Secure file uploads (size limits, scanning)
* Validate all incoming request bodies
* Be explicit about supported Content-Types

