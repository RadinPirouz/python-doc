# FastAPI – POST Requests with File Uploads

This document demonstrates how to handle **file uploads** in FastAPI.
File uploads are essential for APIs that need to receive **images, documents, or binary data** from clients.

---

## Example Application

Create or update `main.py` with the following content:

```python
from fastapi import FastAPI, File, UploadFile, status
from fastapi.responses import JSONResponse
from typing import List

app = FastAPI()

users_db = [
    {"id": "1", "name": "radin", "password": "123"}
]


@app.post("/file")
def upload_file_bytes(file: bytes = File(...)):
    """
    Receive file as raw bytes.
    Returns the size of the uploaded file.
    """
    return {"file_size": len(file)}


@app.post("/uploadfile")
def upload_file_uploadfile(file: UploadFile):
    """
    Receive file as UploadFile.
    Returns filename, content type, and size.
    """
    content = file.read()
    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "file_size": len(content)
    }


@app.post("/uploadmultifile")
def upload_multiple_files(files: List[UploadFile]):
    """
    Receive multiple files as UploadFile list.
    Returns filenames and content types.
    """
    return [
        {"filename": file.filename, "content_type": file.content_type}
        for file in files
    ]
```

---

## File Upload Methods

### 1. `File` as Bytes

* Accepts the uploaded file as raw bytes
* Suitable for small files or direct in-memory processing
* Fast but lacks metadata (filename, content type)

**Example Request (curl):**

```bash
curl -X POST "http://localhost:8000/file" \
  -F "file=@example.txt"
```

**Response:**

```json
{
  "file_size": 128
}
```

---

### 2. `UploadFile`

* Accepts file as `UploadFile` object
* Provides metadata: `filename` and `content_type`
* Supports `.read()`, `.write()`, and `.seek()` operations
* More efficient for large files (uses spooled temporary files)

**Example Request (curl):**

```bash
curl -X POST "http://localhost:8000/uploadfile" \
  -F "file=@example.txt"
```

**Response:**

```json
{
  "filename": "example.txt",
  "content_type": "text/plain",
  "file_size": 128
}
```

---

### 3. Multiple File Uploads

* Accepts a list of `UploadFile`
* Allows uploading multiple files in one request
* Useful for batch uploads or form submissions

**Example Request (curl):**

```bash
curl -X POST "http://localhost:8000/uploadmultifile" \
  -F "files=@file1.txt" \
  -F "files=@file2.txt"
```

**Response:**

```json
[
  {"filename": "file1.txt", "content_type": "text/plain"},
  {"filename": "file2.txt", "content_type": "text/plain"}
]
```

---

## Content-Type

For file uploads, the request must include:

```
Content-Type: multipart/form-data
```

* Each file is sent as a separate part in the multipart request

---

## Running the Application

Start the service using `uvicorn`:

```bash
uvicorn main:app --reload
```

---

## Best Practices

* Use `UploadFile` for large or multiple files
* Validate file type and size on the server
* Avoid loading very large files fully into memory
* Use HTTPS for secure file transfer
* Store files in dedicated storage (S3, local disk, or DB)
* Return clear metadata (filename, size, content type) to clients
* Support multiple files when needed for batch operations
