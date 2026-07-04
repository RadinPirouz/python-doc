# FastAPI – POST Requests with File Uploads

## Prerequisites

* [13 – Async/await](../../fundamentals/13-async-await.md) — `async def`, `await file.read()`
* [12 – HTTP basics](../../fundamentals/12-http-basics.md) — `multipart/form-data`
* [06 – Packages and modules](../../fundamentals/06-packages-modules.md) — `pip install`
* [11-post-types](11-post-types.md) — multipart Content-Type

## Overview

This document explains how to handle file uploads in FastAPI using `POST` requests.

File upload endpoints are commonly used when an API needs to receive:

```text
Images
Documents
PDF files
Text files
Binary files
Multiple files in one request
```

FastAPI supports file uploads using:

```python
File
UploadFile
```

For real applications, `UploadFile` is usually preferred because it provides file metadata and handles large files more efficiently.

---

# 1. Required Package

FastAPI file uploads require `python-multipart`.

Install it with:

```bash
pip install python-multipart
```

If you are using the standard FastAPI installation, it may already be included:

```bash
pip install "fastapi[standard]"
```

---

# 2. Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, File, UploadFile
from typing import List

app = FastAPI()


@app.post("/file")
def upload_file_bytes(file: bytes = File(...)):
    """
    Receive a file as raw bytes.
    Returns the size of the uploaded file.
    """
    return {
        "file_size": len(file)
    }


@app.post("/uploadfile")
async def upload_file_uploadfile(file: UploadFile):
    """
    Receive a file as an UploadFile object.
    Returns filename, content type, and file size.
    """
    content = await file.read()

    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "file_size": len(content)
    }


@app.post("/uploadmultifile")
async def upload_multiple_files(files: List[UploadFile]):
    """
    Receive multiple files as UploadFile objects.
    Returns filenames and content types.
    """
    result = []

    for file in files:
        result.append({
            "filename": file.filename,
            "content_type": file.content_type
        })

    return result
```

---

# 3. File Upload as Bytes

## Endpoint

```python
@app.post("/file")
def upload_file_bytes(file: bytes = File(...)):
    return {
        "file_size": len(file)
    }
```

This endpoint receives the uploaded file as raw bytes.

## Endpoint URL

```http
POST /file
```

## Example Request

```bash
curl -X POST "http://localhost:8000/file" \
  -F "file=@example.txt"
```

## Example Response

```json
{
  "file_size": 128
}
```

## Explanation

```python
file: bytes = File(...)
```

This tells FastAPI to expect a file field named `file`.

The uploaded file is loaded directly into memory as bytes.

## When to Use This Method

This method is suitable for:

```text
Small files
Simple testing
Direct in-memory processing
Quick file size checks
```

## Limitation

This method does not provide file metadata such as:

```text
Original filename
Content type
File headers
```

It also loads the whole file into memory, so it is not ideal for large files.

---

# 4. File Upload with `UploadFile`

## Endpoint

```python
@app.post("/uploadfile")
async def upload_file_uploadfile(file: UploadFile):
    content = await file.read()

    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "file_size": len(content)
    }
```

## Endpoint URL

```http
POST /uploadfile
```

## Example Request

```bash
curl -X POST "http://localhost:8000/uploadfile" \
  -F "file=@example.txt"
```

## Example Response

```json
{
  "filename": "example.txt",
  "content_type": "text/plain",
  "file_size": 128
}
```

---

# 5. Why Use `UploadFile`

`UploadFile` is better than raw bytes for most real APIs.

It provides useful metadata:

```python
file.filename
file.content_type
file.file
```

It also supports file operations such as:

```python
await file.read()
await file.write()
await file.seek()
await file.close()
```

## Important Note

When using `UploadFile.read()`, the endpoint should usually be asynchronous:

```python
async def upload_file_uploadfile(file: UploadFile):
    content = await file.read()
```

This is better than writing:

```python
def upload_file_uploadfile(file: UploadFile):
    content = file.read()
```

because `file.read()` is asynchronous and should be awaited.

---

# 6. Multiple File Uploads

## Endpoint

```python
@app.post("/uploadmultifile")
async def upload_multiple_files(files: List[UploadFile]):
    result = []

    for file in files:
        result.append({
            "filename": file.filename,
            "content_type": file.content_type
        })

    return result
```

## Endpoint URL

```http
POST /uploadmultifile
```

## Example Request

```bash
curl -X POST "http://localhost:8000/uploadmultifile" \
  -F "files=@file1.txt" \
  -F "files=@file2.txt"
```

## Example Response

```json
[
  {
    "filename": "file1.txt",
    "content_type": "text/plain"
  },
  {
    "filename": "file2.txt",
    "content_type": "text/plain"
  }
]
```

## Explanation

```python
files: List[UploadFile]
```

This tells FastAPI to receive multiple uploaded files using the same form field name:

```text
files
```

Each uploaded file is handled as an `UploadFile` object.

---

# 7. Content-Type for File Uploads

File uploads use:

```http
Content-Type: multipart/form-data
```

When using `curl` with `-F`, this header is generated automatically.

Example:

```bash
curl -X POST "http://localhost:8000/uploadfile" \
  -F "file=@example.txt"
```

The request sends the file as a multipart form field.

---

# 8. Complete Recommended Version

```python
from fastapi import FastAPI, File, UploadFile, HTTPException, status
from typing import List

app = FastAPI()

MAX_FILE_SIZE = 5 * 1024 * 1024


@app.get("/")
def root():
    return {
        "message": "API is working"
    }


@app.post("/file")
def upload_file_bytes(file: bytes = File(...)):
    if len(file) > MAX_FILE_SIZE:
        raise HTTPException(
            status_code=status.HTTP_413_REQUEST_ENTITY_TOO_LARGE,
            detail="File is too large"
        )

    return {
        "file_size": len(file)
    }


@app.post("/uploadfile")
async def upload_file_uploadfile(file: UploadFile):
    content = await file.read()

    if len(content) > MAX_FILE_SIZE:
        raise HTTPException(
            status_code=status.HTTP_413_REQUEST_ENTITY_TOO_LARGE,
            detail="File is too large"
        )

    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "file_size": len(content)
    }


@app.post("/uploadmultifile")
async def upload_multiple_files(files: List[UploadFile]):
    result = []

    for file in files:
        content = await file.read()

        if len(content) > MAX_FILE_SIZE:
            raise HTTPException(
                status_code=status.HTTP_413_REQUEST_ENTITY_TOO_LARGE,
                detail=f"File is too large: {file.filename}"
            )

        result.append({
            "filename": file.filename,
            "content_type": file.content_type,
            "file_size": len(content)
        })

    return result
```

---

# 9. Running the Application

Start the FastAPI service using `uvicorn`:

```bash
uvicorn main:app --reload
```

The application will be available at:

```text
http://localhost:8000
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

# 10. Testing with curl

## Upload File as Bytes

```bash
curl -X POST "http://localhost:8000/file" \
  -F "file=@example.txt"
```

## Upload Single File with `UploadFile`

```bash
curl -X POST "http://localhost:8000/uploadfile" \
  -F "file=@example.txt"
```

## Upload Multiple Files

```bash
curl -X POST "http://localhost:8000/uploadmultifile" \
  -F "files=@file1.txt" \
  -F "files=@file2.txt"
```

---

# 11. `bytes` vs `UploadFile`

| Feature                    | `bytes`                     | `UploadFile`              |
| -------------------------- | --------------------------- | ------------------------- |
| File content               | Loaded directly into memory | Uses file-like object     |
| Filename                   | Not available               | Available                 |
| Content type               | Not available               | Available                 |
| Best for                   | Small files                 | Large files and real APIs |
| Metadata                   | No                          | Yes                       |
| Recommended for production | Usually no                  | Yes                       |

---

# 12. Best Practices

Use `UploadFile` for real-world APIs.

Use `bytes` only for small files or simple testing.

Validate file size on the server.

Validate file type before processing or storing the file.

Do not trust the uploaded filename.

Do not store uploaded files directly using user-provided names.

Avoid loading very large files fully into memory.

Use HTTPS for secure file transfer.

Store files in dedicated storage such as:

```text
Local disk
Object storage such as S3 or MinIO
Database storage when appropriate
Network file storage
```

Return clear metadata to the client, such as:

```text
Filename
Content type
File size
Upload status
```
